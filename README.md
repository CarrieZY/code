# code
用于整理学习笔记
## 通用的js类
```
var conf = {
    serverHost : ''
};
var _mm = {
    // 网络请求
    request : function(param){
        var _this = this;
        $.ajax({
            type        : param.method  || 'get',
            url         : param.url     || '',
            dataType    : param.type    || 'json',
            data        : param.data    || '',
            success     : function(res){
                // 请求成功
                if(0 === res.status){
                    typeof param.success === 'function' && param.success(res.data, res.msg);
                }
                // 没有登录状态，需要强制登录
                else if(10 === res.status){
                    _this.doLogin();
                }
                // 请求数据错误
                else if(1 === res.status){
                    typeof param.error === 'function' && param.error(res.msg);
                }
            },
            error       : function(err){
                typeof param.error === 'function' && param.error(err.statusText);
            }
        });
    },
    // 获取服务器地址
    getServerUrl : function(path){
        return conf.serverHost + path;
    },
    // 获取url参数
    getUrlParam : function(name){
        var reg     = new RegExp('(^|&)' + name + '=([^&]*)(&|$)');
        var result  = window.location.search.substr(1).match(reg);
        return result ? decodeURIComponent(result[2]) : null;
    },
    // 渲染html模板
    renderHtml : function(htmlTemplate, data){
        var template    = Hogan.compile(htmlTemplate),
            result      = template.render(data);
        return result;
    },
    // 成功提示
    successTips : function(msg){
        alert(msg || '操作成功！');
    },
    // 错误提示
    errorTips : function(msg){
        alert(msg || '哪里不对了~');
    },
    // 字段的验证，支持非空、手机、邮箱的判断
    validate : function(value, type){
        var value = $.trim(value);
        // 非空验证
        if('require' === type){
            return !!value;
        }
        // 手机号验证
        if('phone' === type){
            return /^1\d{10}$/.test(value);
        }
        // 邮箱格式验证
        if('email' === type){
            return /^(\w)+(\.\w+)*@(\w)+((\.\w{2,3}){1,3})$/.test(value);
        }
    },
    // 统一登录处理
    doLogin : function(){
        window.location.href = './user-login.html?redirect=' + encodeURIComponent(window.location.href);
    },
    goHome : function(){
        window.location.href = './index.html';
    }
};

module.exports = _mm;
```


面试阶段。
1.介绍一下自己
2.做过什么项目？你觉得最好的项目是什么？
3.AJAX 中get和post请求区别？
4.post请求类型有哪些？get和post具体应用场景说下
5.写过响应式布局？讲一下。      媒体查询，百分比，rem，视口viewport
6.讲一下弹性布局？        （弹性布局这个非常重要）
    怎么去实现一排子项目平分父元素       （flex-grow：1）
7.讲一下样式预处理语言         （这个答less ，讲一下变量，样式嵌套，还有一些函数）
8.讲一下工程化工具webpack，怎么用        （答了entry , output, loader, plugin）
9.项目写过ES6，讲讲ES6 
  讲到箭头函数的this
  讲到了异步编程，promise用来解决什么问题？
  讲到JS事件循环机制 event loop？js异步函数有哪些？
   setInterval 和 promise 异步回调有啥区别
   then和catch是回调的吗
10.必问的优化网站       
11.不声明第三个变量情况下，交换两个变量    
12.你用到了哪些前端框架？
13.讲一讲这些框架有什么优缺点？看你的项目用了Angular,谈谈它和React区别？
14.让你去学一个新框架，你怎么去学？
15.如果你发现你的小组里面的人的有错，你会怎么做？
16.怎么看待加班？
--------------------- 
作者：广鑫必为全栈攻城狮 
来源：CSDN 
原文：https://blog.csdn.net/weixin_37823121/article/details/82183118 
版权声明：本文为博主原创文章，转载请附上博文链接！