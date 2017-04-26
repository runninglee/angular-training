# 人人都会的CKEditor
 
 尝试过国内某些富文本编辑器之后，心灰意冷之余，想起了旧伙计CKEditor, 于是决定推一把CKEditor,为的是不要大家把时间花在有意义的地方，
 
 言归正传，走起。


## 下载代码

 [官网](!http://ckeditor.com/download)
 
 [GitHub](!https://github.com/ckeditor/ckeditor-dev)
 
 官网提供三种模式，简洁，标准，高级版，定制版，一般需求不高的话，选择标准版就可以了，有更多的需求可以自定义插件，插件的扩展我在下面细说；
 
 到此为止，根据你的需求下载吧，你可以yarn, npm, bower, git，直接下载，各种方式任你玩。
 
 ## 跑起来
 
 * 拷贝ckeditor到系统可访问的目录下面，一般(主域名／ckeditor),就是资源目录下面了
 
 * 需要配置的话，可以配置一下config.js, 定制CKEditor的样式，插件都是在这儿完成
 
 * 引入ckeditor.js到系统项目中，这个时候CKEditor就已经引入进来了
 
 * 渲染CKEditor到视图中
 
 ```html
 <div style="width: 720px;height: 400px;">
    <textarea name="editor1" id="editor1"  >
        This is my textarea to be replaced with CKEditor.
    </textarea>
 </div>
```

```js

 CKEDITOR.replace( 'editor1' );
     
```

 这个时候不出意外你应该看到了期待的CKEditor,这个时候你已经可以编辑出漂亮的文本了，不满足的往下走着瞧。
 
## 图片上传 

标准版已经自带图片光环，稍加调整即可

1. 开启上传图片功能

修改ckeditor/plugins/image/dialogs/image.js中的hidden值

`
id:'Upload',hidden:true
`

2. 去掉定义预览文本

去掉ckeditor/plugins/image/dialogs/image.js中的image_previewText默认值；

`
image_previewText || ""
`

以上方式都是直接修改插件所为，建议通过修改配置

3. 定义上传服务接口

`
 config.filebrowserUploadUrl = '/api/common/photo/ckeditor';
`

4. 服务器返回回调数据

 ```php
 
 #TODO
 
 ...
 Code Here To Save Image
 ...

 #Return
 $callback, $url, $message
 return '<script>window.parent.CKEDITOR.tools.callFunction('.$callback.', "'.$url.'", "'.$message.'")</script>';

```

不出意外你已经上传成功了

## 自定义插件

CKEditor有大量的插件，可以在官网中自由的下载，集成插件也很容易，下载完插件直接放到ckeditor/plugins/下载的插件，

系统已经自动加载到了，请不要做乱七八糟的事情，管住自己的双手！

## 配置插件

插件本身都有晚上的文档，可以参考文档自定义配置，集成配置是在config.js中设置，把API中的配置集成进来即可

演示一个统计单词熟练的插件配置

```js
CKEDITOR.editorConfig = function (config) {
  // Define changes to default configuration here.
  // For complete reference see:
  // http://docs.ckeditor.com/#!/api/CKEDITOR.config

  // The toolbar groups arrangement, optimized for two toolbar rows.
  config.toolbarGroups = [
    {name: 'clipboard', groups: ['undo']},
    {name: 'insert'},
    '/',
    {name: 'basicstyles', groups: ['basicstyles', 'cleanup']},
    {name: 'paragraph', groups: ['list', 'indent', 'blocks', 'align']},
    {name: 'styles'},
    {name: 'colors'}
  ];

  // Remove some buttons provided by the standard plugins, which are
  // not needed in the Standard(s) toolbar.
  config.removeButtons = 'Underline,Subscript,Superscript';

  // Set the most common block elements.
  config.format_tags = 'p;h1;h2;h3;pre';

  // Simplify the dialog windows.
  config.removeDialogTabs = 'image:advanced;link:advanced';

  // Upload Image Url
  config.filebrowserUploadUrl = '/api/common/photo/ckeditor';

  // Words Count
  config.extraPlugins = 'wordcount,notification';

  config.wordcount = {

    // Whether or not you want to show the Paragraphs Count
    showParagraphs: false,

    // Whether or not you want to show the Word Count
    showWordCount: false,

    // Whether or not you want to show the Char Count
    showCharCount: true,

    // Whether or not you want to count Spaces as Chars
    countSpacesAsChars: false,

    // Whether or not to include Html chars in the Char Count
    countHTML: false,

    // Maximum allowed Word Count, -1 is default for unlimited
    maxWordCount: -1,

    // Maximum allowed Char Count, -1 is default for unlimited
    maxCharCount: 5000,

    // Add filter to add or remove element before counting (see CKEDITOR.htmlParser.filter), Default value : null (no filter)
    filter: new CKEDITOR.htmlParser.filter({
      elements: {
        div: function( element ) {
          if(element.attributes.class == 'mediaembed') {
            return true;
          }
        }
      }
    })
  };
};

```

你会了吗？看到这，你可以肆无忌惮的封装了，Angular,Vue, React喜欢那个来那个！


#License

[MIT License](https://opensource.org/licenses/mit-license.html). ©  [Running Lee](mailto:lihui870920@gmail.com)