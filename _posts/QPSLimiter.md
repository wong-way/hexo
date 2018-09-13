---
title: QPSLimiter
date: 2018-08-23 22:38:05
categories: java
tags: 
- QPSlimiter
- redis
- 分布式
- Nginx负载均衡
- 令牌桶算法
description: 基于Nginx做负载均衡，redis做缓存与共享数据的分布式限流工具
---
# QPSLimiter

## 0.背景
频次限制（rate limiting）是Web系统比较常见的功能，防止用户频繁访问接口，导致系统负载增加而影响服务的质量

## 1.思路
参照Google的Guava中RateLimiter的设计，采用令牌桶算法实现本次功能[https://en.wikipedia.org/wiki/Token_bucket]。令牌桶算法是指随着时间流逝,系统会按恒定1/QPS时间间隔(如果QPS=100,则间隔是10ms)往桶里加入Token(想象和漏洞漏水相反,有个水龙头在不断的加水),如果桶已经满了就不再加了.新请求来临时,会各自拿走一个Token,如果没有Token可拿了就阻塞或者拒绝服务.

## 2.功能实现

```java
public class RateLimiter {
    private final int permits;
    private final long durationInMillis;
    private final Object mutex = new Object();
    private volatile int left;
    private volatile long expires;
    public RateLimiter(int permits, long durationInMillis) {
        this.permits = permits;
        this.durationInMillis = durationInMillis;
        this.left = permits;
        this.expires = durationInMillis + System.currentTimeMillis();
    }
    public boolean tryAcquire() {
        tryReset();     //过期后及时重置
        return left > 0 && doAcquire();
    }
    private boolean doAcquire() {
        synchronized (mutex) {
            if (left > 0) {
                left -= 1;
                return true;
            }
        }
        return false;
    }
    private boolean tryReset() {
        if (shouldReset()) {
            synchronized (mutex) {
                if (shouldReset()) {
                    this.left = permits;
                    expires = System.currentTimeMillis() + durationInMillis;
                }
                return true;    //并发时可能由其他线程重置成功，只要重置了就返回true
            }
        }
        return false;
    }
    private boolean shouldReset() {
        return expires < System.currentTimeMillis();
    }
}

```
## 3.分析
上面这种问题存在几个问题
* 在实际线上环境中，所有调用方的请求都会该程序，如果程序崩溃，那么整个系统就不能正常运行。
* 同时，令牌存放在内存中，如果程序崩溃，在1s中能够顺利重启，但是之前记录的某个调用方当前的请求数量已经丢失，可能存在QPS超过限制的情况
* 频次限制应该统一存储webserver是任意可拓展个数的服务如果将rate limiter的存储放在了各自服务中，导致明明我限制的是整个集群这个API的访问单人不可超过5QPS，实际上却是5*服务数量QPS。所以这个频次限制需要统一存储，统一整个系统的访问频次。

## 4.改进——分布式限流
拟准备部署多台服务器或docker，并将调用方的令牌存放到redis集群中，所有的服务器或docker均从redis中读取令牌信息，同时使用Nginx做一个反向代理以及负载均衡。

## 5. 代码实现
```java
@Service
public class RedisService {

    private final static Logger logger = Logger.getLogger(RedisService.class.getName());
    private final String suffix = ":max";
    private final StringRedisTemplate template;

    @Autowired
    public RedisService(StringRedisTemplate template) {
        this.template = template;
    }


    public Response setCallerQps(String callerName, int num) {
        Response response = new Response();
        Info info = new Info();
        try {
            template.opsForValue().set(callerName + suffix, Integer.toString(num));
            info.setCode(OPERATION_SUCCESS);
            info.setMsg("插入记录成功");
        } catch (Exception e) {
            info.setMsg("插入记录失败");
            info.setCode(SYSTEM_ERROR);
            logger.error("插入记录失败" + e.getMessage(), e);
        }
        response.setInfo(info);
        return response;
    }

    public Response isExceeded(String caller) {
        Response response = new Response();
        Info info = new Info();

        /* 完成逻辑
         */
        String QPSObj = template.opsForValue().get(caller + suffix);
        if (QPSObj == null) {
            logger.error("未知调用方：" + caller);
            info.setMsg("服务商未注册");
            info.setCode(Constant.CALLER_NOT_EXIST);
        } else {
            int maxQPS = Integer.parseInt(QPSObj);
            //已知调用方
            long count;
            count = template.opsForValue().increment(caller, 1);
            if (count == 1) {
                //不含有该调用方，说明已过期，重置时间片
                template.expire(caller, 1, TimeUnit.SECONDS);
            }
            if (count > maxQPS) {
                //调用方超过QPS
                logger.info(caller + "超过限制");
                info.setMsg("超过限制");
                info.setCode(Constant.QPS_EXCEEDED);
            } else {
                info.setMsg(caller + "没有超过限制");
                info.setCode(Constant.QPS_NOT_EXCEEDED);
            }
        }
        response.setInfo(info);
        return response;
    }
}

```
项目代码地址放到我的github中

## 总结
* 针对线上的功能，实现对指定对象有访问频次的限制
* http方式支持多个客户端访问
* 低延迟
* 承受较大的访问量
* 服务易于拓展