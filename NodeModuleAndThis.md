# 通过实例了解module.exports exports 和 this
起因是 [Node社区一个交流帖子](https://cnodejs.org/topic/5b508f3caef62f1b0f9e0485)，当时以为是简单作用域问题，仔细了解运行环境后才知道涉及不少方面的问题。

关键内容为以下几点：
- js脚本文件在node环境下执行的时候，this指向的是**module.exports所指向的对象**
- js脚本文件被引用时，结果为module.exports
- let var 声明与全局作用域的关系

test.js
-
<pre>
    module.exports.x = 'original x';
    console.log(this.x);    // 'original x'
    console.log(module.exports === this);   // true;

    module.exports = {x: 'modified x'};
    console.log(module.exports === this);   // false


    var _obj = {
        x: '_obj x',
        arrowFunc: () => {
            console.log(this.x);
        },
        normalFunc: () => {
            console.log(this.x);
        }
    };

    _obj.arrowFunc();   // 'original x'
    _obj.normalFunc();  // '_obj x'
</pre>
node 环境下执行
<pre>
    > var result = require('test.js');
    > result // {x: 'modified x'};
</pre>