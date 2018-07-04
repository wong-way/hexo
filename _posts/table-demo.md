---
title: table demo
date: 2017-05-07 11:29:57
categories: Java Web
tags: 
- java web
- java
description: html创建一个表格示例
---

```html
<table class="table table-bordered table-striped" id="msg">
  <thead>
    <tr>
      <th>#</th>
      <th>title</th>
      <th>create day</th>
      <th>categories</th>
      <th>comment number</th>
      <th>operation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <th>database concept</th>
      <th>2017-05-06</th>
      <th>Comp.Sci.</th>
      <th>132</th>
      <th>
        <a href="javascript:void( alert('编辑'))"
           class="btn btn-primary btn-sm waves-effect waves-light m-b-5"><i
                                                                            class="fa fa-edit"></i> <span>编辑</span></a>
        <a href="javascript:void( alert('删除'))"
           class="btn btn-danger btn-sm waves-effect waves-light m-b-5"><i
                                                                           class="fa fa-trash-o"></i> <span>删除</span></a>
        <a href="javascript:void( alert('预览'))"
           class="btn btn-warning btn-sm waves-effect waves-light m-b-5" 
           target="_blank"><i
                              class="fa fa-rocket"></i> <span>预览</span></a>
      </th>
    </tr>
    <tr>
      <th>2</th>
      <th>software</th>
      <th>2015-02-06</th>
      <th>Comp.Sci.</th>
      <th>432</th>
      <th>
        <a href="javascript:void( alert('编辑'))"
           class="btn btn-primary btn-sm waves-effect waves-light m-b-5"><i
                                                                            class="fa fa-edit"></i> <span>编辑</span></a>
        <a href="javascript:void( alert('删除'))"
           class="btn btn-danger btn-sm waves-effect waves-light m-b-5"><i
                                                                           class="fa fa-trash-o"></i> <span>删除</span></a>
        <a href="javascript:void( alert('预览'))"
           class="btn btn-warning btn-sm waves-effect waves-light m-b-5" 
           target="_blank"><i
                              class="fa fa-rocket"></i> <span>预览</span></a>
      </th>
    </tr>
  </tbody>
</table>
```

