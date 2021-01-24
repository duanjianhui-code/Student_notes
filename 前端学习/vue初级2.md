#  vue（初级2）

## vue的组件化

1、如何使用组件

> 创建组建构造器
>
> 注册组建
>
> 使用组建

```html
<script>
    //1.创建组件构造器
   const cpnC = Vue.extend({
     template :`
    <div>
      <h2>我是标题</h2>
    <p>我是南乳 发深V会司法考试</p>
    <p>我是南乳 发深V会司法考试</p>
  </div>`
    })
   //2、注册组件
   Vue.component('my-cpn',cpnC)
   
</script>

//使用组件。在boby标签中使用
<my-cpn></my-cpn>
```

template:用于定义组件模版的内容



## vue全局组件与局部组件

上述定义的组件为全局组件，可以在多个vue实例下使用；

而vue的局部组件则定义在创建vue实例中

```html
<body>
  <div id="app">
    <my-cpn></my-cpn>
    <cpn></cpn>
  </div>
  <script src="../js/vue.js"></script>
<script>
  //1.创建组件构造器
   const cpnC = Vue.extend({
     template :`
    <div>
      <h2>我是标题</h2>
    <p>我是南乳 发深V会司法考试</p>
    <p>我是南乳 发深V会司法考试</p>
  </div>`
    })
  //2.注册组件，此方法创建的为全局组件
 Vue.component('my-cpn',cpnC)

  const app=new Vue({
    el:'#app',
    data:{},
    methods:{},
    //注册的为局部组件；cpn为标签名，cpnC为组件构造器
    components:{
    cpn:cpnC
    }
  })
</script>
```

## 父组件与子组件

```html
<body>
  <div id="app">
    <cpn2></cpn2>
  </div>
<script>
  //创建组件构造器1（子组件）
  const cpnC1=Vue.extend({
    template:`
    <div>
      <h2>我是标题1</h2>
      <h2>我是内容，哈哈哈哈</h2>
    </div>`
  })
  //创建组件构造器2（父组件）
  const cpnC2=Vue.extend({
    template:`
    <div>
      <h2>我是标题2</h2>
      <h2>我是内容，呵呵呵</h2>
      <cpn1></cpn1>
    </div>`,
  components:{
    //将组件1注册到组件2中
    cpn1:cpnC1
    }
  })

  const app=new Vue({
    el:'#app',
    data:{},
    methods:{},
    components:{
      //将组件2挂在到vue实例上
      cpn2:cpnC2
    }
  });
</script>
```

如果要在vue挂载的实例下使用<cpn1></cpn1>标签，还需要在vue的components下注册组件cpnC1

```html
<script>
	const app=new Vue({
    el:'#app',
    data:{},
    methods:{},
    components:{
      //将组件2挂在到vue实例上
      cpn2:cpnC2,
      cpn1:cpnC1
    }
  });
</script>
```

注册组件的语法糖

```html
 //1、全局组件注册的语法糖
  //注册组件
  Vue.component('cpn1',{
    template:`
      <h2>你好！！！</h2>
    `
  })
//2、局部组件注册语法糖
  const app=new Vue({
    el:'#app',
    data:{},
    methods:{},
    components:{
      cpn2:{
        template:`
      <h2>你好啊！！！</h2>
    `
      }
    }
  });
```

语法糖省略了Vue.extend()创建组件构造器的方法，简化了书写，只要在注册组件的时候将｛template｝代替组件构造器，其原理还是会去调用Vue.extend()方法来创建组件构造器；

## 组件模板分离

> 1、使用<script></script>
>
> 2、使用<template></template>

```html
<body>
  <div id="app">
    <cpn1></cpn1>
  </div>
<!-- 1、使用script标签，注意：类型必须为："text/x-template" -->
<script type="text/x-template" id="cpn">
  <div>
    <h2>我是标题</h2>
    <p>我是内容，哈哈哈</p>
  </div>
</script>
<!-- 2、template -->
<template id="cpn1">
  <div>
    <h2>我是标题</h2>
    <p>我是内容，哈哈哈ghghhghjghjhjhjg</p>
  </div>
</template>
<script>
  //注册一个全局组件
  Vue.component('cpn1',{
    template:
     '#cpn1'
  })
```

组件不可以访问vue实例数据，就是vue的data{}的数据

> 组件是一个单独功能模块的封装，有着自己的data属性，data必须是一个函数，函数返回一个对象，对象保存着数据

```html
<!-- 定义一个模板-->
<template id="cpn1">
    <div>
      <h2>{{title}}</h2>
      <p>{{content}}</p>
    </div>
  </template> 
//注册一个全局组件
<script>
  Vue.component('cpn1',{
    template:
     '#cpn1',
     data(){
       return {
         title:'标题',
         content:'我是内容'
       }
     }
  })
</script>
```

组件计数器的示例：组件内部自己维护了data,methods

```html
<body>
  <div id="app">
    <cpn1></cpn1>
  </div>

  <template id="cpn1">
    <div>
     <h2>计数器：{{counter}}</h2>
     <button @click="increment">+</button>
     <!-- :disabled="counter<1"在counter为0的时候，disabled属性失效 -->
     <button @click="decrement" :disabled="counter<1">-</button>
    </div>
  </template>
<script>
  //注册一个全局组件
  Vue.component('cpn1',{
    template:
     '#cpn1',
    data(){
      return {
        title:'标题',
        content:'我是内容',
        counter: 0
      }
     },
    methods:{
      increment(){
        this.counter++
      },
      decrement(){
        this.counter--
      }
    }
  })
  const app=new Vue({
    el:'#app',
    data:{},
    methods:{}
  });
</script>
</body>
```

## 父子组件的通信

在开发中，数据往往是从上层传递到下层：

> 比如在一个页面中，我们从服务器请求到了很多数据
>
> 其中的一部分数据，并非整个页面的大组件来展示的，而是需要下面的子组件来展示数据
>
> 在这时，并不是让子组件去发送网络请求，而是通过大组件（父组件）将数据传递给小组件（子组件）

如何实现父子组件的通信：

>1、父组件--->子组件：props
>
>2、子组件-->父组件：通过自定义事件

### 1、父传子：

方式一：通过props选项使用一个数组

```html
<body>
  <div id="app">
    <cpn :cmovies="movies" :cmessage="message"></cpn>
  </div>
  <template id="cpn">
    <div>
      <ul>
        <li v-for="movie in cmovies">{{movie}}</li>
      </ul>
      <p>{{cmessage}}</p>
    </div>
  </template>
<script>
  //子组件
  const cpn={
    template:'#cpn',
    props:['cmovies','cmessage'],
    data(){
      return {}    
    },
    methods: {},
  }
  const app=new Vue({
    el:'#app',
    data:{
      movies:['海王','dsds','sdswe','232','sdaw3434'],
      message:'hello'
    },
    methods:{},
    //注册局部组件
    components:{
      cpn
    }
  });
</script>
</body>
```

方式二:使用对象的方式，次方法可以验证数据的类型

> String Number Boolean Array Object Date Function Symbol

```html
<script>
props:{
      //1、类型限制
      //cmovies:Array,
      //cmessage:String
      //2、提供一些默认值以及毕传值
      cmessage:{
        type:String,
        default:'aaaaaaaa',
        //
        required: true
      },
      //类型是对象或者数组时，默认值必须是一个函数
      cmovies:{
        type:Array,
        default(){
          return []
        }
      }
    }
</script> 
```

### 2、子传父：

通过自定义事件来传递：在子组件中$emit('自定义事件的名字'，参数)；在父组件中通过v-for来监听子组件事件

```html
<body>
  <div id="app">
    <cpn @item-click="cpnClick"></cpn>
    <p>{{categories}}</p>
  </div>

  <!--子组件模板-->
  <template id="cpn">
    <div>
      <button v-for="item in categories" @click="btnClick(item)">{{item.name}}</button>
    </div>
  </template>
<script>
  //子组件
  const cpn={
    template:"#cpn",
    data(){
      return {
        categories:[
          {id:'aaa',name:'热门推荐'},
          {id:'bbb',name:'手机'},
          {id:'ccc',name:'电脑'},
          {id:'ddd',name:'零食'}
        ]
      }
    },
    methods: {
      btnClick(item){
        //发送事件
        this.$emit('item-click', item);
      }
    },
  }
  //父组件
  const app=new Vue({
    el:'#app',
    data:{
      categories:[]
    },
    methods:{
      cpnClick(item){
        this.categories=item
      }
    },
    components:{
      cpn
    }
  });
</script>
</body>
```

![image-20210120210925280](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20210120210925280.png)



## 父子组件组件通信案例

```html
<body>
  <div id="app">
    <cpn :number1="num1" :number2="num2" 
    @number1change="num1charge"
    @number2change="num2charge"></cpn>
  </div>

  <template id="cpn">
    <div>
    <h2>props{{number1}}</h2>
    <h2>data{{dnumber1}}</h2>
    <!-- <input type="text" v-model="dnumber1"></input> -->
    <input type="text" :value="dnumber1" @input="num1Input"></input>
    <h2>props{{number2}}</h2>
    <h2>data{{dnumber2}}</h2>
    <!-- <input type="text" v-model="dnumber2"></input> -->
    <input type="text" :value="dnumber2" @input="num2Input"></input>
  </div>
  </template>
<script>
  const cpn={
    template:'#cpn',
    props:{
      number1:Number,
      number2:Number
    },data(){
      return {
        dnumber1:this.number1,
        dnumber2:this.number2
      }
    },
    methods: {
      num1Input(event){
        //将input的value赋值到dnumber1
        this.dnumber1=event.target.value;
        //将dnumber1的值传给父组件
        this.$emit('number1change', this.dnumber1);
        //同时修饰dnumber2的值
        this.dnumber2=this.dnumber1*100;
        //将被改变的dnumber2的值传给父组件
        this.$emit('number2change', this.dnumber2); 
      },
      num2Input(event){
        this.dnumber2=event.target.value;
        this.$emit('number2change', this.dnumber2);
        this.dnumber1=this.dnumber2/100;
        //将被改变的dnumber2的值传给父组件
        this.$emit('number1change', this.dnumber1); 
      }
    },
  }
  
  const app=new Vue({
    el:'#app',
    data:{
      num1:1,
      num2:0
    },
    methods:{
      num1charge(value){
        this.num1=parseFloat(value)
      },
      num2charge(value){
        this.num2=parseFloat(value)
      }
    },
    components:{
      cpn
    }
  })
</script>
</body>
```



## 父子组件组件的访问：

### 1、父访问子

> 通过$children,此为一个数组，要通过下标的索引来获取对应的属性和方法
>
> 通过$refs

```html
<!-- 方式一 -->
<body>
  <div id="app">
    <cpn ref="aaa"></cpn>
    <button @click="showmessage">按钮</button>
  </div>

  <template id="cpn">
    <div>
      <p>我是子组件</p>
    </div>
  </template>
<script>
  const cpn = {
    template:'#cpn',
    data(){
      return{
      name:'段建辉'
      }
    },
    methods: {
      showMessage(){
        console.log('showmessage'+this.name)
      }
    },
  }
  const app=new Vue({
    el:'#app',
    data:{},  
    methods:{
      showmessage(){
        //通过$chilfren,实际开发中用的是$refs
        // this.$children[0].showMessage();
        console.log(this.$refs.aaa.name);
      }
    },
    components: {
      cpn
    }
  });
</script>
</body>
```

### 2、子访问父

> $parent
>
> $root

```html
<body>
  <div id="app">
    <cpn></cpn>
  </div>

  <template id="cpn">
    <div>
      <p>我是子组件</p>
      <button @click="showMessage">按钮</button>
    </div>
  </template>
<script>
  const cpn = {
    template:'#cpn',
    data(){
      return{
      name:'段建辉'
      }
    },
    methods: {
      showMessage(){
        //访问父组件
        console.log('showmessage'+this.$parent.name);
        //访问root
        console.log(this.$root.name);
      }
    },
  }
  const app=new Vue({
    el:'#app',
    data:{
      name:'上搭建爱仕达'
    },  
    methods:{}
    },
    components: {
      cpn
    }
  });
</script>
</body>
```

## vue组件化高级

### slot的使用

![image-20210124134953826](C:\Users\duanjianhui\AppData\Roaming\Typora\typora-user-images\image-20210124134953826.png)

```html
<body>
  <div id="app">
    <cpn></cpn>
    <cpn><input type="text"></input></cpn>
    <cpn></cpn>
    <cpn></cpn>
  </div>

  <template id="cpn">
    <div>
      <h2>我是子组件</h2>
      <p>ghhhhh</p>
      <!--如果<cpn></cpn>没有添加标签，就会默认使用<slot></slot>中的标签 -->
      <slot><button>按钮</button></slot>
    </div>
  </template>
<script>
  const cpn={
    template:'#cpn',

  }
  const app=new Vue({
    el:'#app',
    data:{},
    methods:{},
    components:{
      cpn
    }
  });
</script>
</body>
```

具名插条：

```html
<body>
  <div id="app">
    <cpn><span slot="center">标题</span></cpn>
    <cpn><button slot="left">按钮</button></cpn>
  </div>

  <template id="cpn">
    <div>
      <!--如果<cpn></cpn>没有添加标签，就会默认使用<slot></slot>中的标签 -->
      <slot name="left"><span>左边</span></slot>
      <slot name="center"><span>中间</span></slot>
      <slot name="right"><span>右边</span></slot>
    </div>
  </template>
</body>
```

### 作用域插槽

```html
<body>
  <div id="app">
    <cpn></cpn>
    <cpn>
      <!-- 在2.5以上的版本可以不用<template></template> -->
      <template slot-scope="slot">
        <span>{{slot.data.join(' - ')}}</span>
      </template>
    </cpn>
  </div>

  <template id="cpn">
    <div>
      <slot :data="PLanguages">
        <ul>
         <li v-for="item in PLanguages ">{{item}}</li>
        </ul>
      </slot>
    </div>
  </template>
<script>
  const cpn={
    template:'#cpn',
    data(){
      return {
        PLanguages : ['java','python','c++','c#','objectc']
      }
    }
  }
  const app=new Vue({
    el:'#app',
    data:{},
    methods:{},
    components:{
      cpn
    }
  });
</script>
</body>
```

