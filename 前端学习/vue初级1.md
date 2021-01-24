# vue（初级1）

## 一、什么是vue

Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与[现代化的工具链](https://cn.vuejs.org/v2/guide/single-file-components.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#libraries--plugins)结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

## 二、安装

通过cdn引入

```html
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>

```

通过引入js文件引入

```html
<script src="../js/vue.js"></script>
```

通过·安装



## 三、Vue初体验

```html
<body>
    <div id="app">{{message}}
        <h1>{{name}}</h1>
    </div>

<script src="../js/vue.js"></script>
<script>
    //let(变量)、const(常量)
    const app=new Vue({
        el:'#app',//el定义挂载的元素
        data:{   //定义数据
            message:'你好，段建辉',
            name:'code'
        }
    })
</script>
</body>
```

结果：

![image-20201222105504082](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201222105504082.png)

## 四、Vue的基本语法

### 1、插值语法

#### mustache语法

数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值：

```html
表达式为：
{{}}

例子：
<div id="app">
    <!--mustache语法中，不仅仅可以直接写变量，也可以写简单的表达式-->
    <h1>{{message}}</h1>
    <h1>{{message}},段建辉</h1>
    <h1>{{firestName+lastName}}</h1>
    <h1>{{count*2}}</h1>
  </div>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        message: '哈喽！',
        firestName: 'kobo',
        lastName: 'toy',
        count: 100
      },
      methods: {}
    });
  </script>
```

结果：

![image-20201222141226188](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201222141226188.png)



#### v-once

通过使用 [v-once 指令](https://cn.vuejs.org/v2/api/#v-once)，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上的其它数据绑定：

```html
<body>
  <div id="app">
    <h1>{{message}}</h1>
    <h1 v-once>{{message}}</h1>
  </div>
<script>
  const app=new Vue({
    el:'#app',
    data:{
      message: '哈喽！'
    },
    methods:{}
  });
</script>
</body>
```

#### v-html

双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 v-html,

```html
<body>
  <div id="app">
    <h1 v-html="url">{{url}}</h1>
  </div>
<script>
  const app=new Vue({
    el:'#app',
    data:{
      url: '<a href="www.baidu.com">百度一下</a>'
    },
    methods:{}
  });
</script>
```

结果：

![image-20201222142935109](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201222142935109.png)

#### v-pre

将标签里的数据原封不动的显示出来，不进行解析加载

```html
<h1 v-pre>{{message}}</h1>

显示的结果为：
{{message}}

在未加载v-pre时，结果为
data:{
  message: 'hello'
}的值
```



#### v-cloak

在vue解析前，div中有一个属性v-cloak，解析后就不存在了，此属性可以解决在js未加载完成之前不然用户看到未经过处理的元素和数据

```html
定义div中的元素不可见
<style>
    [v-cloak] {
      display: none;
    }
</style>

使用v-cloak
<div id="app" v-cloak>
    {{message}}
</div>
```

### 2、动态绑定

#### v-bind-动态绑定属性

v-bind用于绑定一个或者多个属性值，比如动态绑定a元素的href属性或者img的src属性

```html
<div id="app"><img v-bind:src="url">
<a v-bind:href=""></a>
</div>

 data: {
        url:'https://img14.360buyimg.com/n0/jfs/t1/129944/2/9049/332241/5f505247E6cb51b19/edae501e948935d3.jpg'},

v-bind的简写-----:  语法糖的表示方法
<img :src="url">
```

##### v-bind绑定class(对象)

![image-20201222154655866](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201222154655866.png)

```html
<body>
  <div id="app">
    <h2 :class="{active: isActive, line: isLine}">{{message}}</h2>
    <h2 :class="getclass()">{{message}}</h2>
    <button @click="btnclick">按钮</button>
  </div>
  <script src="../js/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        message: '你好啊！！！',
        isActive: true,
        isLine: true
      },
      methods: {
        btnclick: function () {
          this.isActive = !this.isActive;
        },
        getclass: function () {
            retuen {active: this.isActive, line: this.isLine};
        }
      }
    });
  </script>
</body>
```

##### v-bind绑定class（数组）

引用的少

```html
<body>
  <div id="app">
    <h2 :class="getClasses()"></h2>
  </div>
<script>
  const app=new Vue({
    el:'#app',
    data:{
      message: '你好！！！',
      active: 'aaaaa',
      line: 'bbbb'
    },
    methods:{
      getClasses: function () {
        return [this.active, this.line];
      }
    }
  });
</script>
</body>
```

##### v-bind动态绑定style（对象）

```html
<body>
  <div id="app">
    <!--<h2 :style="{key(css的属性名):value(属性值)}">{{message}}</h2>-->
    <h2 :style="getStyle()">{{message}}</h2>
  </div>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        message: '你好',
        finalColor: 'red',
        finalSize: '100px'
      },
      methods: {
        getStyle: function () {
          return {fontSize: this.finalSize,color: this.finalColor};
        }
      }
    });
  </script>
</body>
```

##### v-bind动态绑定style（数组）



#### v-for遍历

```html
<body>
  <div id="app">
    <ul>
      <li v-for="m in movies">{{m}}</li>
    </ul>
  </div>
<script>
  const app=new Vue({
    el:'#app',
    data:{
      movies: ['海王','海尔兄弟','代码笔记']
    },
    methods:{}
  });
</script>
</body>
```

结果:

![image-20201222161302463](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201222161302463.png)

### 3、vue的计算属性

```html
<body>
  <div id="app">
    <h2>总价格：{{totalPrice}}</h2>
  </div>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        book:
          [
            { id: 110, bookname: 'java入门到精通', price: 24 },
            { id: 111, bookname: 'java入门到精通1', price: 57 },
            { id: 112, bookname: 'java入门到精通2', price: 67 },
            { id: 113, bookname: 'java入门到精通3', price: 100 }
          ]
      },
      //计算属性
      computed: {
        totalPrice: function () {
          let result = 0;
          for (let i = 0; i < this.book.length; i++) {
            result+=this.book[i].price;
          }
          return result;
        }
      }
    });
  </script>
</body>
```



#### 计算属性的setter和getter

```html
<body>
  <div id="app"><h2>{{fullname}}</h2></div>
<script>
  const app=new Vue({
    el:'#app',
    data:{firstname: 'toy',
      lastname: 'rose'},
    methods:{ },
    computed: {
      fullname: {
        get: function () {
          return this.firstname + ' ' + this.lastname;
        }
      }
    }
  });
</script>
</body>
```

计算属性一般没有set方法，只读属性

将上述代码简化后：

```html
<script>
  const app=new Vue({
    el:'#app',
    data:{firstname: 'toy',
      lastname: 'rose'},
    methods:{ },
    computed: {
      fullname: function () {
          return this.firstname + ' ' + this.lastname;
        }
    }
  });
</script>
```

#### 计算属性与methods的对比

注意：methods和computed看起来实现了相同的功能，为什么还要使用计算属性呢？

原因：计算属性会进行缓存，如果多次使用，计算属性只会使用一次

### 4、事件监听

v-on 为事件监听，如点击事件为v-on:click,可以简写为@click;

```html
<body>
    <div id="app">
        <h1>当前计数：{{count}}</h1>
        <button @click="add">+</button>
        <button @click="sub">-</button>
    </div>

    <script src="../js/vue.js"></script>
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                count: 0
            },
            methods: {
                add: function () {
                    this.count++;
                },
                sub: function () {
                    this.count--;
                }
            },
        })
    </script>
</body>
```

v-on参数问题：

```html
<body>
  <div id="app">
    <!--事件调用方法没有带参数-->
    <button @click="btnClick">按钮1</button>
    <button @click="btnClick()">按钮2</button>
    <!--在事件定义时，写函数时省略了小括号，但本身是需要一个参数的-->
    <!--在方法定义时，我们需要event对象，同时还需要其他参数，event的参数表达式为：$event,-->
    <button @click="btn3Click('duanjianhui',$event)">按钮3</button>
    <button>按钮4</button>
  </div>
  <script>
    const app = new Vue({
      el: '#app',
      data: {},
      methods: {
        btnClick() {
          alert("btnclick");
        },
        btn3Click(name, event) {
          console.log('++++++++++' + name + event)
        }
      }
    });
  </script>
</body>
```

v-on的修饰符问题：

.stop:存在事件冒泡问题，vue通过修饰符来解决了这个问题

.prevent:阻止默认行为,例如表单默认提交的行为



```html
<body>

<div id="app">
  <!--1. .stop修饰符的使用-->
  <div @click="divClick">
    aaaaaaa
    <button @click.stop="btnClick">按钮</button>
  </div>
  <!--2. .prevent修饰符的使用-->
  <br>
  <form action="baidu">
    <input type="submit" value="提交" @click.prevent="submitClick">
  </form>

  <!--3. .监听某个键盘的键帽-->
  <input type="text" @keyup.enter="keyUp">

  <!--4. .once修饰符的使用-->
  <button @click.once="btn2Click">按钮2</button>
</div>

<script src="../js/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊'
    },
    methods: {
      btnClick() {
        console.log("btnClick");
      },
      divClick() {
        console.log("divClick");
      },
      submitClick() {
        console.log('submitClick');
      },
      keyUp() {
        console.log('keyUp');
      },
      btn2Click() {
        console.log('btn2Click');
      }
    }
  })
</script>

</body>
```

### 5、条件判断

#### v-if ：

v-if后面的条件为false时，对应的元素和子元素都不会渲染，也就是不会出现在DOM中

```html
<h2 v-if="true"></h2>
```

#### v-else:

当v-if为false时，执行v-else

```html
  <div id="app">
    <h2 v-if="isShow">
      {{message}}
    </h2>
    <h1 v-else>isShow为false时, 显示我</h1>
  </div>
```

#### v-else-if

```html
 <div id="app">
    <h2 v-if="score >= 90">优秀</h2>
    <h2 v-else-if="score >= 80">良好</h2>
    <h2 v-else-if="score >= 70">一般</h2>
    <h2 v-else="score >= 60">及格</h2>
  </div>
```

结果：

![image-20201223124044667](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201223124044667.png)

#### 条件渲染案例

用户再登录时，可以切换使用用户账号登录还是邮箱地址登录。

![image-20201223131344670](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201223131344670.png)

```html
<body>
  <div id="app">
    <span v-if="isUser">
      <label for="username">用户账号</label>
      <input type="text" id="username" placeholder="用户账号" key="username">
    </span>
    <span v-else>
      <label for="email">用户邮箱</label>
      <input type="text" id="email" placeholder="用户邮箱" key="email">
    </span>
    <button @click="changestatus">切换登录</button>
  </div>
<script>
  const app=new Vue({
    el:'#app',
    data:{
      isUser: true
    },
    methods:{
      changestatus(){
        this.isUser= !this.isUser;
      }
    }
  });
</script>
</body>

```

#### v-show

v-show的用法和v-if非常相似，也用于决定一个元素是否渲染

v-if和v-show对比:

> v-if当条件为false时，压根不会有对应的元素在DOM中
>
> v-show当条件为false时，仅仅是将元素的display属性设置为none而已

开发中如何选择呢:

> 当需要在显示与隐藏之间切片很频繁时，使用v-show
>
> 当只有一次切换时，通过使用v-if

```html
<div v-show="true">
    <h1>{{message}}</h1>
</div>
```

### 6、循环遍历

#### v-for

v-for的语法类似于JavaScript中的for循环，格式如下：item in items的形式；

如果在遍历的过程中不需要使用索引值

```html
<li v-for="movie in movies"></li>
//依次从movies中取出movie，并且在元素的内容中，我们可以使用Mustache语法，来使用movie
```

如果在遍历的过程中，我们需要拿到元素在数组中的索引值

```html
<li v-for="(item, index) in items"></li>
//其中的index就代表了取出的item在原数组的索引值
```

示例1：遍历数组

```html
<body>
  <div id="app">
    <ul>
      <li v-for="(name,index) in names">{{index+1}}  {{name}}</li>
    </ul>
  </div>
<script>
  const app=new Vue({
    el:'#app',
    data:{
      names: ['tom' ,'jery','rose','dover']
    },
    methods:{}
  });
</script>
</body>
```

结果：

![image-20201223133556641](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201223133556641.png)

示例2：遍历对象

```html
 <div id="app">
    <ul>
      <!--1、在遍历对象时，如果只获取一个值，那么回去的值是value-->
      <li v-for="info in infos">{{info}}</li>
    </ul>

    <ul>
      <!--2、获取key和value (value,key)-->
      <li v-for="(value,key) in infos">{{key}} {{value}}</li>
    </ul>
  </div>
```

v-for 的key 属性:提高性能

![image-20201223140459884](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201223140459884.png)

```html
<li v-for="itea in iteas" key="itea">{{itea}}</li>
```

#### 小练习：

1、点击是变成红色

```html
 <style>
    .active{
      color: red;
    }
  </style>
</head>

<body>
  <div id="app">
    <ul>
      <li v-for="(mover,index) in movies" :class="{active: index==currentIndex}" @click="clickred(index)">{{mover}}</li>
    </ul>
  </div>
<script>
  const app=new Vue({
    el:'#app',
    data:{
      movies:['海王','猫和你说','美国队长','复联'],
      currentIndex: 0
    },
    methods:{
      clickred(index){
        this.currentIndex=index;
      }
    }
  });
</script>
</body>
```

![image-20201223161128997](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201223161128997.png)

**图书购物车**

实现如下需求：

![image-20201223161254880](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201223161254880.png)

```html
 <div id="app">
   <div v-if="books.length">
    <table>
      <thead>
        <tr>
          <th></th>
          <th>书籍名称</th>
          <th>出版日期</th>
          <th>价格</th>
          <th>购买数量</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item,index) in books">
          <td>{{item.id}}</td>
          <td>{{item.name}}</td>
          <td>{{item.date}}</td>
          <!--采用方法的形式进行保留两位小数-->
          <!--<td>{{getFinalPrice(item.price)}}</td>-->
          <!--采用过滤器的形式进行保留两位小数-->
          <td>{{item.price | shoePrice}}</td>
          <td>
            <button @click="add(index)">+</button>
            {{item.count}}
            <button @click="sub(index)" :disabled="item.count<=1">-</button>
          </td>
          <td>
            <button @click="removeClick(index)">移除</button>
          </td>
        </tr>
      </tbody>
    </table>
    <h2>总价：{{totalPrice | shoePrice}}</h2>
   </div>
   <h2 v-else>购物车为空</h2>
  </div>
```

```javascript
const app = new Vue({
  el: '#app',
  data: {
    books: [{
        id: 1,
        name: '《算法导论》',
        date: '2006-9',
        price: 85.00,
        count: 1
      },
      {
        id: 2,
        name: '《UNIX编程艺术》',
        date: '2006-2',
        price: 59.00,
        count: 1
      },
      {
        id: 3,
        name: '《编程珠玑》',
        date: '2008-10',
        price: 39.00,
        count: 1
      },
      {
        id: 4,
        name: '《代码大全》',
        date: '2006-3',
        price: 128.00,
        count: 1
      },
    ]

  },
  methods: {
    getFinalPrice(price){
      return '￥' +price.toFixed(2);
    },
    add(index){
      this.books[index].count++;
    },
    sub(index){
      this.books[index].count--;
    },
    removeClick(index){
      this.books.splice(index,1);
    }
  },
  //过滤器
  filters: {
    shoePrice(price){
      return '￥' +price.toFixed(2);
    }
  },
  //计算属性
  computed: {
    totalPrice(){
      let result=0;
      for (let i=0;i<this.books.length;i++){
        result+=this.books[i].price*this.books[i].count;
      }
      return result;
    }
  }
})
```

```css
table {
  border: 1px solid #e9e9e9;
  border-collapse: collapse;
  border-spacing: 0;
}

th, td {
  padding: 8px 16px;
  border: 1px solid #e9e9e9;
  text-align: left;
}

th {
  background-color: #f7f7f7;
  color: #5c6b77;
  font-weight: 600;
}
```

![image-20201223173751656](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20201223173751656.png)

### 7、v-model的使用：

实现双向绑定

```html
  <div id="app">
    <input type="text"  v-model="messgae">
  {{messgae}}
  </div>
<script>
  const app=new Vue({
    el:'#app',
    data:{
      messgae:'你好呀！'
    },
    methods:{}
  });
</script>
```

其实现原理：动态绑定value-->v-bind:value;监听input事件-->v-on:input;​

```html
  <div id="app">
    <input type="text" :value="message" @input="getchange"></input>
    {{message}}
  </div>
<script>
  const app=new Vue({
    el:'#app',
    data:{
      message:'你好！'
    },
    methods:{
      getchange(event){
        this.message=event.target.value;
      }
    }
  });
</script>
```

v-model与radio的使用

```html
  <label for="male">
      <input type="radio" id="male" v-model="sex" value="男">男</input>
    </label>
    <label for="female">
      <input type="radio" id="female"  v-model="sex" value="女">女</input>
    </label>
    <h2>你选择的是:{{sex}}</h2>
  </div>
<script>
  const app=new Vue({
    el:'#app',
    data:{
      sex:'男'//设置默认值
    },
    methods:{}
  });
</script>
```

v-model的属性使用：

![image-20210110213919772](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20210110213919772.png)

```html
<body>
  <div id="app">
    <!-- 修饰符 lazy -->
    <input type="text"  v-model.lazy="messgae">
  {{messgae}}

  <!-- 修饰符 number -->
  <input type="text"  v-model.number="age">
  {{age}} -->{{typeof age}}

  <!-- 修饰符 trim -->
  <input type="text"  v-model.trim="name">
  你的名字：{{name}}
  </div>
<script>
  const app=new Vue({
    el:'#app',
    data:{
      messgae:'你好呀！',
      age:0,
      name:''
    },
    methods:{}
  });
</script>
```

## 

## 五、高阶函数的使用

for循环的使用：

```html
方法一：
for(let i = 0; i<arrays.length; i++){
    console.log(arrays[i]);                           
}
                                
方法二：
for(let i in arrays){
   console.log(array[i]);
}

方法三：
for(item of arrays){
	console.log(item);                               
}

```

高阶编程函数：

filter:回调函数必须返回一个boolean值，当返回为true时，函数的内部会自动将本次回调的n加入到新的数组中，为false时，函数内部就会将其过滤掉这次的n

示例：

```html
const nums= [10,24,45,78,67,100,200];
let newnums=nums.filter(function(n){
	return n<100;
});
结果为：[10,24,78,67]
```

map函数的使用：回调函数必须返回一个值，它会将处理好的新的值加入到一个新的数组里去

```html
const nums= [10,24,45,78,67,100,200];
let newnums=nums.map(function(n){
	return n*2;
});
结果为：[20,48,90,156,134,200,400]
```

reduce:对数组中的所有内容进行汇总

回调函数的preValue为上一次回调函数返回的值

```html
const nums= [10,100,200];
let total=nums.reduce(function(preValue,n){
	return preValue+n;
},0);
//第一次：preValue 0    n  10
//第二次：preValue 110  n  100
//第三次：preValue 310  n  200
```

