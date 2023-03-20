## eslint自定义插件

[参考地址](https://github.com/eslint/eslint)

----

## 通过.eslintrc.js文件进行eslint配置

```js
  module.exports = {
    //可以扩展配置,@group/eslint-config-a，可以省略eslint-config</font>
    "extends":['@group/a'],
    //可以自定义规则插件，@group/eslint-plugin-a,可以省略eslint-plugin
    "plugins":['@group/a'],
    //配置提示等级
    "@group/a/no-use-method":'error'
  }
```
## 自定义插件
```js
  module.exports = {
    rules:{
      "no-use-method":require('./no-use-method')({
        regex:/(^\s*|\.)(method\()/,
        description:'method was scrapped',
        errorTips:'method方法被废弃，请使用newMethod方法'
      })
    }
  }
```
## 代码实现
./no-use-method文件
```js
  module.exports= ({regex,description,errorTips})=>{
    return{
      meta:{
        type:'suggestion',
        docs:{
         description:description,
         recommended:false
        },
        schema:[],
        messages:{
         errorTips:errorTips
        }
      },
      create(context) {
        constsourceCode=context.getSourceCode();
        return{
         Program(node) {
          sourceCode.getLines().forEach((line,index)=>{
           let match;
           if((match=regex.exec(line)) !==null) {
             context.report({
               node,
               loc:{
                 start:{
                   line:index+1,
                   column:match.index
                 },
                 end:{
                   line:index+1,
                   column:match.index+match[0].length
                 }
               },
               messageId:'errorTips'
             });
           }
         });
        }
       };
      }
     };
    };
  ```

