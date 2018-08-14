## 水木 BBS 二站版面抓取 JS 代码演示

本段代码用于从水木社区二站（ 2.newsmth.net） 的公开版面抓取所有帖子，归档为一个 .html 页面文件。

具体用法如下：
1. 先用浏览器打开要抓取的版面（例如 C.abc）并转到第一页的帖子列表，然后在控制台运行下列代码；
2. 手工点击第一个帖子，然后程序就会按顺序逐个打开帖子，并将其内容存入 page 字符串；
3. 浏览完成后，控制台会输出 page 字符串，把它复制粘贴保存为一个 .html 文件，打开就是该版面的所有帖子了。
同时，浏览帖子的那个 iframe 里也会显示抓取到的所有帖子内容。

*【注意】此工具没有在一站测试过，它只是作为动态加载 iFrame 并提取其中 DOM 节点方法的代码演示，使用者请注意谨慎使用，切勿违反有关法律法规。*

```javascript
var iframe = document.getElementsByName('f3')[0];    //获取显示帖子内容的 iframe
//初始化抓取结果，它是一个 HTML 页面，因此先把头部以及简单的几个样式放进去
var page = '<!DOCTYPE html><html><head><title>SMTH</title><style>div {border-top: 1px solid yellowgreen; padding: 10px; color: royalblue;}</style></head><body>';
//点击打开第一个帖子就可以激活下面的函数执行
iframe.onload = function(){
    var innerDoc = iframe.contentDocument || iframe.contentWindow.document;  //获取 iframe 里的 document 对象
    var post = innerDoc.getElementsByClassName('article')[0].innerHTML;      //获取帖子内容对应的 HTML 元素
    page += '<div>'+post.replace(/src="/g, 'src="http://www.2.newsmth.net/')+'</div>';      //把当前的帖子内容加入抓取结果的 HTML 页面里
    var next = innerDoc.getElementsByClassName('conPager smaller right')[0].children[1].href;  //获取下一个帖子的链接地址
    if(iframe.src != next) {            //判断是否抓取完成
        iframe.src = next;      //未完成，把 iframe 的 src 属性变为下一条帖子的链接地址
    }
    else{
        page += '</body></html>';   //已完成，给结果 HTML 页面填上结尾标签
        innerDoc.write(page);       //把 iframe 里的内容替换为结果 HTML 页面
        console.log(page);          //在控制台输出结果 HTML 页面的内容，以便用户在编辑器里复制保存为 HTML 文件
    }
}
```
