##   VUE知识点
1. 迭代式开发

2. 
   配置对象
   属性名固定

3. VUE性能好

4. 原生的和jquery他们都获取一个dom而且
   在修改一个dom都会重绘一遍

5. vue和原生及jQuery是不一样的,他采用文档碎片()把整个dom树的重绘从多次变成一次

6. 在内存中更新

7. 在for循环中修改p标签 , 循环完在渲染

8. vm对象和传递的配置对象不是同一个对象

9. 数据代理: 使用vm代理了配置对象当中data的数据,vm身上也有和data当中的同名的属性,模板当中访问的都是vm身上的属性
   vm代理了data当中的数据,找vm获取数据最终还是拿的data当中的属性值
   修改vm的数据其实本质是在修改data

10. v-bind:href = "url"
    一旦用了,指令那么这个被指令使用的属性,属性值就是js代码了,而原本的""不在代表字符串的边界

11. v-on:click= "test"

12. :href="url"
    单向数据绑定指令的简写

13. 
    @click
    添加事件监听的简写

14. 双向数据绑定  一般情况下只针对表单元素才使用
    v-model="msg"
    双向程  三线程

15. m:modle 就是我们的数据
    v: view 就是我们的页面
    vm: vue的实例化对象

16. mvvm 说的就是双向数据绑定模型
    双数据绑定: 数据可以从data 流向页面 页面的数据被更新,也会从页面流向data
    当data的数据被更改以后,又会重新流向页面
    
17. el:'#app', 一般情况挂载都是书写配置
    .$mount('#app') 第二种写法
    Objext.dfineProperty方法
    在为对象添加或者修改  属性为响应式属性
    响应式你变我也变 响应式会自动变

18. firstName  姓
    lastName   名

19. obj.fullName   全名

20. 在vue中所有的this都指向vm,因为这些方法或者函数都会被vm代理

21. computed
    当我需要一个数据,但是这个数据我又没有,并且这个数据由目前我有的数据计算而来.那就要用计算属性
    
22. 使用方法去获取姓名和计算属性去计算姓名的区别
    对于方法调用
    你是用了几次方法调用,那么这个方法被调用几次
    对于计算属性
    你使用不管几次计算属性,计算属性的get方法只调用一次
    计算属性一定存在缓存,这样有缓存使用多次的时候效率比使用方法高得多

23. 
    监视的数据一定是存在的,而且是可以变化的

24. computed函数当中只能使用同步,而watch当中可以是同步也可以是异步

25. 我们去比较computed和watch 的时候起始比较的是计算属性的get方法

26. 计算属性的set没什么说的,其实仅仅是对计算机的属性添加了监视(当计算属性的值被修改之后,会调用set)

27. ####  Vue生命周期

28. ```vue
    new Vue({
                el:"#app",
                data() {
                    return {
                        isShow:true
                    }
                },
                methods: {
                    destory(){
                        this.$destroy()
                    }
                },
                beforeCreate() {
                    // 初始化之前,打印不到
                    console.log(this,this.isShow)
                },
                created() {
                    //初始化之后能访问
                    console.log(this.isShow)
                },
                beforeMount() {
                    // 挂载阶段 
                    console.log(this.$refs.pp)
                },
                mounted() {
                    //发送ajax请求
                    console.log(this.$refs.pp)
                    this.timer = setInterval(()=>{
                        this.isShow = !this.isShow
                    },2000)
                },
                beforeUpdate() {
                    //页面更新前  
                    console.log(this.isShow,this.$refs.pp.innerHTML)
                },
                updated() {
                    //页面更新后  
                    console.log(this.isShow,this.$refs.pp.innerHTML)
                },
                //销毁
                beforeDestroy() {
                    //销毁之前
                    clearInterval(this.timer)
                    this.timer = null
                },
                destroyed() {
                    //销毁后
                    console.log('销毁')
                },
            })
    ```

    
    this.timer = null
彻底清除定时器

在那个组件中写的this,this就指向那个组件

组件对象.$emit
组件对象.$on

vm和组件对象的关系
 vm实例化对象的原型是组件对象原型的原型

$on $emit 等方法是在vm的隐式原型身上的 (Vue的显示原型)

vm原型对象就是组件对象的原型对象的原型对象


#### vue中添加过度和动画

1.理解进入和离开    一共六个类

2.那个元素在变化   就给那个原始使用transition (vue给我们定义的一个子组件标签) 标签

3.组件标签使用的时候,name决定类名





v-for="(comment,index) in coms"

遍历列表渲染

​      :key="comment.id"

渲染对比

​      :comment="comment"

传评论

​      :deleteComment="deleteComment"

传删除方法

​      :index="index"

索引



#### 脚手架

两步骤

先实现静态页面,在实现交互

静态页面三步拆组件,组装组件,渲染组件

动态页面三步



组件组装三步

引入 注册 使用



全局事件总线

本质就是一个对象

两个条件:

1.必须让所有组件对象都可以看见(找到)

2这个对象必须能使用$on $emit 方法

在vue当中,都是选择vm作为全局事件总线

A

事件最终绑定在总线上回调函数留在A当中

B

最终触发的是总线身上的事件

B的数据触发时也会被传递







A组件

在A当中可以获取到bus绑定事件this.$bus.$on,回调函数留在了绑定事件的地方(A)回调函数在那个组件 ,那个组件就是为了接收数据



B组件

在B当中也可以获取到bus触发事件this.$bus.$emit,在B当中触发总线身上的事件B当中写的触发,B就是传递数据的一方

插槽

默认插槽

slot内部的东西 是等待着父组件使用的时候给传过来

具名插槽 

要起一个名字



无论是默认插槽还是具名插槽,里面都有可以写东西,也可以不写东西

如果写了东西







axios是根据 xhr和promise封装而来的一个http库

http请求  包括普通http请求和ajax请求

我们用来发送ajax请求最多的工具就是axios

引入之后暴露给我们一个函数  也是函数对象

axios 	有两种用法  函数用法和对象用法

axios()  	函数

axios.get() 函数对象的用法

请求方式:一般普通的http请求就是get 和post

ajax 请求  get   查   post   增 	put     改   delete 删

项目几乎都是前后端分离

前端做前端的 (搭页面.做交互 (请求获取数据展示渲染))

后端做后端的 (数据库的存储操作,接口的书写 , 服务器的部署)

restful  Api  接口规范  是现在目前最流行的书写后端接口的一个规范

资源化

每个数据都会被看作是一个资源(商品)  	对于每一个资源来说操作数据库有四个情况 增	删	改	查

后端人员对数据库的每个操作  对应前端人员 ajax的4个请求

ajax请求

get  调用接口  查找数据资源

post  调用接口  添加数据资源

put   调用接口   修改数据资源

delete  调用接口    删除数据资源



   params  参数是路径的一部分,并且这个参数只能在url路径当中出现

​	query  这个参数是查询参数,这个参数可以出现在url   当中,也可以在配置项当中配置,query参数不属于路径的一部分

在url当中 是?后面的   key = value& key = value,

在配置项当中   配置项的名称叫 params

body 请求体参数 

 通常用在post 和put  当中,只能 在配置对象当中配置

data  这个配置就是你的body请求体参数, 这个必须是一个对象

如果是对象写法,请求方式是对象的方法名,第一个参数都代表url (params和query都直接写在url当中)

如果有body请求体参数,post和put  第一个参数代表的就是请求体参数

get  delete 第二个参数不是请求体参数,代表是一个配置对象

修改页面显示的状态数据,为了让页面显示正在搜索

forEach   map   filter    some     every  reduce   都暗含遍历

功能 :   加工数组   根据已有的数据创建新的数组 ,  新数组 当中每一项和老数组当中每一项对应有关系

参数:  回调函数  (item,index,arr) 每个数组项都会执行这个回调函数,返回的是一个新的项,放进新数组当中

返回值:  把每一项都返回的新项组成的新数组返回

async   await		是使用同步代码实现异步效果

async 		函数代表这是一个异步函数	async 函数返回的是promise

async 函数返回值不看return  必然返回promise

async 函数返回的promise是成功还是失败  看return 

retur 的结果代表promise是成功还是失败

1. 如果return 的是一个非promise的值   代表成功
2. 如果返回是成功的promise   代表async函数返回的promise也是成功的   (他们不是同一个promise)



跨域  是浏览器上的同源策略

请求分为http请求和ajax请求



npm  i  vuex      -S

   

state是一个包含多个属性（不是方法）的对象，其实就是用来存储数据用的

const state = {}

 mutations也是一个对象，是一个包含了多个方法的对象，其实就是用这个里面的方法去直接操作数据的

这个里面的方法不能包含if for异步，是直接操作的

 actions也是一个对象，是一个包含了多个方法的对象。这个对象内部的方法是用来和vue当中用户的操作去关联的

const actions = {}

getters也是一个对象 ,是一个包含了多个方法的对象.这个对象内部的每个方法对应了一个计算机属性的get ,就是通过state 当中的已有数据  计算出来的一个新的想要使用的属性数据