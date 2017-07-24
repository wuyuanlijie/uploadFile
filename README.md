# html5+js拖拽和点击上传

## 1. 效果展示
 > 文件上传说起来简单，但其实也不简单，里面涉及的东西很多。
 ![Alt text](https://raw.githubusercontent.com/wuyuanlijie/ImageFile/master/uploadGif.gif)

## 2. 言归正传
上传文件的方式有很多种，我这里只介绍两种点击上传和拖放上传。由于目前没有学习如何将文件数据传到服务器，
我这里就只展示文件的预览效果。

## 3. 技术框架
之前不了解什么是前端框架,如今使用到了，豁然开朗，并且让我从切页面中解脱出来！现在技术更新很快，但是这两个框架让我很受用，或许以后会接触到更多新技术、新框架，却不会忘记它们如何让我们爱上前端工作。两个
1. Bootstrap:[切页面你可以更快](http://www.bootcss.com/)简洁、直观、强悍的前端开发框架，让web开发更迅速、简单。
2. JQuery教程:[优秀的JavaScript代码库](http://www.w3school.com.cn/jquery/)“write Less，Do More”链式调用减少标签的声明。<br>

## 4. 拖拽上传
![Alt text](https://github.com/wuyuanlijie/ImageFile/blob/master/upload.png)
先在body声明一个div类名为dragFile，便于后面js绑定drag事件,并且创建一个盒子list-drag来放置文件预览效果。
```html
    <div class="col-md-5 col-md-offset-1 up-content  dragFile">
      <p style="margin-top:10px;">拖拽图片到这里哟</p>
      <div class="up-area">
        <div class="row">
          <ul class="list-group clearfix list-drag">
          </ul>
        </div>
      </div>

    </div>
 ```
 1. 通过querySelector获取diagFile元素（只有将图片拖放到这个范围才有效），我们在为它添加drop、dragover与dragenter事件。
 2. dragover是当某被拖动的对象在另一对象容器范围内拖动时触发此事件，因为默认情况下，数据不能放在到其他元素上，要实现该功能，我要需要
 调用event.preventDefault()方法来实现dragover。
 3. drop事件在选取文件位置放置在目标区域时触发，同时我们也要在这里阻止默认事件添加上述的方法。并且通过e.dataTransfer.files
 来获取拖拽上传的文件列表信息
 4. 这里我运用到es6的知识，强大的for-of循环，遍历文件数组，并且window.URL.createObjectURL(file)
 将根据传入的文件参数创建一个指向该参数对象的URL。es6果然很难厉害，利用模板字符串[深入浅出ES6（四）：模板字符串](http://www.infoq.com/cn/articles/es6-in-depth-template-string)可以多行书写，模板字符串中所有的空格、新行、缩进，
 都会原样输出在生成的字符串中，然后一个append()方法就可以将文档信息插入到ul下。
 
 ```javascript
   $(".dragFile").on("dragenter", function(e){
          e.preventDefault();
      });
   $('.dragFile').on('dragover', (e) => {
     e.preventDefault();
   })
   $('.dragFile').on('drop', (e) => {
     e.stopPropagation();
     e.preventDefault();
     var files = e.dataTransfer.files; //获取文件
     appendFile(files, '.list-drag')
   })

    function appendFile (files, listName) {
      for( file of files ) {
        let url = window.URL.createObjectURL(file);
        let liStr = `
          <li class="list-group-item">
            <div>
              <img src="${url}" alt="文件" />
            </div>
          </li>
        `;
        $(listName).append(liStr);
      }
    }

 ```
 ## 5. 点击上传
 >这里我就直接po出代码，相信大家很容易看的懂！<br>
 
 Wait！还是讲讲一个我认为值得学习的东西,因为系统自定义的input标签很丑，所以我们一般都会display:none将它隐藏，然后创建一个button按钮来绑定它
 Bootstrap有很多按钮的样式我们可以直接用它的。牛逼的jQuery拥有它的链式调用强大武器，我们可以检索到按钮点击后，直接绑定input的点击事件。
 ```html
  <div class="col-md-5 up-content btnFile">
    <div class="btn">
      <button type="button" class="btn btn-success" id="btn"> 本地上传文件</button>
      <input type="file" style="display:none;" id="fileInput" name="fileselect" multiple>
    </div>
    <div class="up-area">
      <div class="row">
        <ul class="list-group clearfix list-btn">
        </ul>
      </div>
    </div>
  </div>
 ```

 ```javascript
  $('#btn').click( () => {
      $('#fileInput').click();
   })
  $('#fileInput').change( (event) => { //change事件件在上传确定文件后触发
    var files = event.target.files;
    appendFile(files, '.list-btn');
   })

  function appendFile (files, listName) {
    for( file of files ) {
      let url = window.URL.createObjectURL(file);
      let liStr = `
        <li class="list-group-item">
          <div>
            <img src="${url}" alt="文件" />
          </div>
        </li>
      `;
      $(listName).append(liStr);
    }
   }    
 ```
## 项目地址：
https://github.com/wuyuanlijie/UploadFiles

 
> <br>最后如果您觉得本文如有什么写的错误的，大家请指出错误，共同进步！
