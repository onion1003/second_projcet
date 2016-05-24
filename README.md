#第五章笔记

* 学习了第一个“平稳退化”的例子

    <a href="http://www.example.com/" onclick="popUp(this.href);return false;">Example</a>   
在js代码无效时，依然可以工作

* 分离js，即在用外部js操作内部html

在外部js文件中使用 element.event = action

    var links = document.getElementsByTagName("a");
    for (var i=0; i<links.length; i++) {
        if(links[i].getAttribute("class") == "popUp") {
            links[i].onclick = function() {
                popUp(this.getAttribute("href"));
                return false;
            }
        }
    }

👆的代码因为第一句中有document的存在，如果页面没有加载完全的话，就会出错。因为js文件加载时就会立即执行，不管是放在head中还是／body前面。

👇的代码中因为window对象在html文档全部加载后才会触发，而document对象是window对象的一个属性。window触发onload时，document已经存在。

    window.onload = prepareLinks;
    function prepareLinks(){
        var links = document.getElementsByTagName("a");
        for (var i=0; i<links.length; i++) {
            if(links[i].getAttribute("class") == "popup") {
                links[i].onclick = function() {
                    popUp(this.getAttribute("href"));
                    return false;
                }
            }
        }
    }

    function popUp(winURL) {
        window.open(winURL,"popup","width=320,height=480")
    }//打开一个新窗口，第一个参数是链接，第二个是窗口名，第三个是窗口大小


* 向后兼容

使用逻辑非语句会更简单，千万不能在测试语句中加(),否则测试的是语句的结果

    window.onload = function() {
        if(!document.getElementsByTagName) return false;
        var links = document.getElementsByTagName("a");
        for (var i=0; i<links.length; i++) {
            if(links[i].getAttribute("class") == "popup") {
                links[i].onclick = function() {
                    popUp(this.getAttribute("href"));
                    return false;
                }
            }
        }
    }

* 性能考虑

尽量少访问dom和减少标记
利用dom查询元素，浏览器会搜索整棵dom树,标记太多树会变大

    if(document.getElementsByTagName("a").length > 0) {
        var links = document.getElementsByTagName("a");
        for(var i=0; i<links.length; i++) {
            //
        }
    }

👆的代码会搜索两次dom树

👇的只会搜索一次

    var links = document.getElementsByTagName("a");
    if(links.length > 0) {
        for(var i=0; i<links.length; i++){
            //
        }
    }

尽量把js文件都写在一个文件里，放在/body前面，然后使用工具压缩，使用.min.js
