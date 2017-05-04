---
layout: post
title: html - 浏览器中删除file文件域
category: html
tags:
  - js
  - input
---

# 问题

浏览器安全域的限制，不允许直接清除file控件

# 解决

创建一个新的替换原来的

```javascript

// 创建input
createInput : function ()
{
    var _this = this, input = this.toElement('<input type="file" />');

    $(input).attr({
        'class' : 'file-input',
        'name' : 'aws_upload_file',
        'multiple' : this.options.multiple ? 'multiple' : false
    });

    $(input).change(function()
    {
        _this.addFileList(this);
    });

    return input;
}

// 替换原来的
var old_file = $('input[name="aws_upload_file"]');
var new_file = _this.createInput();
old_file.after(new_file);
old_file.remove();

```




