# dist-warp
简单的将单个CommonJS代码包裹成前端代码。
## 安装

`npm install dist-warp`

## API
### warp(source)
### warp(source, globalName)
必须在源文件中包含一行 `module.exports = someModule`。
比如源`source`:
```js
var hello = 'hello';
module.exports = hello; //此行是必须的。
```
执行
```js
var warp = require('dist-warp');
warp(source);
```
结果
```js
(function(){
//############## TOP ############## by dist-wrap

  var hello = 'hello';
  //module.exports = hello;

//############## BOTTOM ############## by dist-wrap
  if (typeof define === 'function' && define.amd) {
    define(function() {
      return hello;
    });
  }else if(typeof module === 'object' && module.exports){
    module.exports = hello;
  }else{
    this.hello = hello;
  }
})();
```
如果想要绑定到全局的名字不一样，你可以用第二个参数：
```js
warp(source, 'HELLO');
```
