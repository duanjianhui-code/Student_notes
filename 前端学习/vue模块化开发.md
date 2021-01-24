# vue模块化开发

## ES6模块化

模块对导出方式：

案例：html文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<script src="aaa.js" type="module"></script>
<script src="bbb.js" type="module"></script>
<script src="mmm.js" type="module"></script>
<body>

</body>
</html>
```

aaa.js文件,模块的导出

```javascript
var name = '小明'
var age = 18
var flag = true

function sum(num1,num2){
  return num1 + num2
}

if (flag) {
  console.log(sum(20,30))
}

//将数据导出方式一
export {
  flag,sum
}

//将数据导出方式二
export var num=1000

//导出函数、类
export function num1(num1,num2){
  return num1 + num2
}

export class Person {
  run (){
    console.log('我是一个类')
  }
}

const address='北京市'

//export default 在同一个模块中只能使用一次
export default address
```

mmm.js：模块的使用--导入

```javascript
import {flag,sum} from "./aaa.js" 
import {num} from "./aaa.js"
import {num1} from "./aaa.js"
import {Person} from "./aaa.js"
import addr from "./aaa.js"

if (flag) {
  console.log('xiaoming,hhhhhhhhhh')
  console.log(num)
  console.log(num1(10,30))
  const p =new Person();
  p.run();
  console.log(addr)
}

//统一导入
import * as aaa from "./aaa.js"

console.log(aaa.flag,aaa.num)
```

