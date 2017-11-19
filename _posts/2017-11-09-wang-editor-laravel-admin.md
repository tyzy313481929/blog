---
layout: post
title: wang-editor在laravel-admin中的使用
categories: PHP
description: wang-editor在laravel-admin中的使用
keywords: wang-editor,laravel-admin
---

在使用laravel-admin二次开发中，用到了wang-editor,使用过程如下。


1、下载wang-editor文件项目下的public/vendor/wangEditor-3.0.10，下载地址<http://www.wangeditor.com/>

2、编辑app/Admin/Extensions/Form/WangEditor.php

```php
<?php

namespace App\Admin\Extensions\Form;

use Encore\Admin\Form\Field;

class WangEditor extends Field
{
    protected $view = 'admin.wang-editor';

    protected static $css = [
        '/vendor/wangEditor-3.0.10/release/wangEditor.css',
    ];

    protected static $js = [
        '/vendor/wangEditor-3.0.10/release/wangEditor.js',
    ];

    public function render()
    {
        $name = $this->formatName($this->column);
//        $token = csrf_token();
        $this->script = <<<EOT

var E = window.wangEditor
var editor = new E('#{$this->id}');
editor.customConfig.uploadImgShowBase64 = true
editor.customConfig.uploadImgServer = '/admin/upload'
editor.customConfig.debug = true
editor.customConfig.uploadFileName = 'wang-editor-image-file'

editor.create()

EOT;
        return parent::render();
    }
}

```
3、配置app/Admin/bootstrap.php
```php
<?php

/**
 * Laravel-admin - admin builder based on Laravel.
 * @author z-song <https://github.com/z-song>
 *
 * Bootstraper for Admin.
 *
 * Here you can remove builtin form field:
 * Encore\Admin\Form::forget(['map', 'editor']);
 *
 * Or extend custom form field:
 * Encore\Admin\Form::extend('php', PHPEditor::class);
 *
 * Or require js and css assets:
 * Admin::css('/packages/prettydocs/css/styles.css');
 * Admin::js('/packages/prettydocs/js/main.js');
 *
 */
use App\Admin\Extensions\Column\ExpandRow;
use App\Admin\Extensions\Column\OpenMap;
use App\Admin\Extensions\Column\FloatBar;
use App\Admin\Extensions\Column\Qrcode;
use App\Admin\Extensions\Column\UrlWrapper;
use App\Admin\Extensions\Form\WangEditor;
use App\Admin\Extensions\Nav\Links;
use Encore\Admin\Grid;
use Encore\Admin\Form;
use Encore\Admin\Facades\Admin;
use Encore\Admin\Grid\Column;

Encore\Admin\Form::forget(['map', 'editor']);
Form::extend('editor', WangEditor::class);

```
4、配置存储路径config/wang-editor.php
```php
<?php

return [
    'upload' => [
        'disk' => 'public',
        'path' => date('Y-m-d'),
        'name' => 'wang-editor-image-file'
    ],
];
```
5、创建view
```html
<div class="form-group {!! !$errors->has($label) ?: 'has-error' !!}">

    <label for="{{$id}}" class="col-sm-2 control-label">{{$label}}</label>

    <div class="{{$viewClass['field']}}">

        @include('admin::form.error')

        <div id="{{$id}}" style="width: 100%; height: 100%;">
            <p>{!! old($column, $value) !!}</p>
        </div>

        <input type="hidden" name="{{$name}}" value="{{ old($column, $value) }}" />

    </div>
</div>
```
6、调用扩展
```php
 protected function form()
    {
        return Admin::form(Schools::class, function (Form $form) {
            $form->display('id', 'ID');
            $form->editor('content');
           
        });
    }
```
7、上传图片
```php
public function upload(Request $request)
    {
        $upload_disk = config('wang-editor.upload.disk', 'public');
        $upload_path = config('wang-editor.upload.path', '');
        if ($request->hasFile('wang-editor-image-file')) {
            $path = $request->file('wang-editor-image-file')->store('wang-editor-file' . $upload_path, $upload_disk);
            $result = [
                'errno' => 0,
                'data'  => [asset('storage/' . $path)]
            ];
        } else {
            $result = ['errno' => 1];
        }
        return response()->json($result);
    }
```
8、注意


* 屏蔽csrftoken,否则提示519错误

* php artisan storage:link创建从storage/public中的软链接
## 参考

* <https://github.com/tonyski/laravel-wang-editor>

* <http://www.jianshu.com/p/8cf60e0fc47e>
