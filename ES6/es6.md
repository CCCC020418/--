      ## class



      类的数据类型是函数,类本身就指向构造函数,

      prototype对象的constructor属性,直接指向类的本身,类的内部所有定义的方法都是不可以枚举的

      constructor 方法是类的默认方法,通过new命令生成对象实例时,自动调用该方法.

      一个类必须有constructor方法,如果没有显式定义,一个空的constructor方法会被默认添加

      ```javascript
      class Point {
      }

      // 等同于
      class Point {
      constructor() {}
      }
      ```

      定义了一个空的类point,JavaScript引擎会自动为他添加一个空的constructor方法

      constructor方法默认返回实例对象(即this),完全可以指定返回另外一个对象

      ```javascript
      class Foo {
      constructor() {
      return Object.create(null);
      }
      }

      new Foo() instanceof Foo
      // false
      ```

      constructor   

      ```javascript
      //实例化对象//
      const onePlus = new  SmartPhone('小米10',4299,'128G','一个亿');

      console.log(onePlus);
      ```

      ```javascript
      //对象继承
      //子类   extends(延伸,扩展,延长)  继承  多态
      class SmartPhone  extends  phone{
      //构造方法
      constructor(brand,price, storage,pixel){
      //调用父类的构造方法 属性初始化
      super(brand, price); 
      this.storage = storage;
      this.pixel =pixel;
      }
      //子类对象的方法
      playGame(){
      console.log('我可以用来玩游戏');
      }
      takePhoto(){
        console.log('可以拍照');
      }
      }
      ```


      ```javascript
      let
      let 声明变量
      字母数字下划线,首字母不能为数字,严格区分大小写,不能使用关键字   驼峰
      let  声明特点
      1.不允许重复声明
      2.块儿级作用域 
      if(){}   else(){}   for()P{}    {}  function(){}
      3.不存在变量提升
      4.不影响作用域链

      let 实践案例
      获取元素对象
      let  items = document.querySelectorAll('.item');

      遍历并绑定事件
      for(let i=0; i<items.length;i++){
      绑定事件
      items[i].onclick = function(){
      items[i].style.background = 'pink';
      }
      }
      const 定义常量
      const  用来声明一个常量(值不变) let a
      0.格式
      const  a = 100;
      1.声明的时候一定要赋初始值
      const PLAYER ='UZI';
      2.常量的名称一般为大写      潜规则
      const RESOLVED ='resolved'
      3.不能修改常量的值
      PLAYER = '小狗'
      4.不允许重复声明
      const  PLAYER = 'adc'
      5.块儿级作用域
      {
      const A = 100;
      }
      console.log(A);
      6.关于数组和对象的元素的修改
      const TEAM = ['UZI','MLXG','LETME']
      TEAM.push('XIYE');
      console.log(TEAM);
      const BIN = {
      name: 'BIN'
      }
      BIN.name = '阿斌';
      consol.log(BIN);
      变量解构赋值
      结构赋值
      const arr = ['宋小宝'.'刘能','赵四','小沈阳']
      let [song,liu,zhao,xiao] = arr;
      console.log(song,liu,zhao,xiao);


      对象解构赋值
      const star = {
      name:'于谦'
      tags:['抽烟','喝酒','烫头']
      say:function(){
      console.log('我可以说相声');
      }
      }
      let {name,tags:[chou,he,tang],say} = star;
      //console.log(name);
      //console.log(tags);
      //console.log(say);
      console.log(chou)
      console.log(he)
      console.log(tang)


      //模板字符串
      //1.直接使用换行符
      let str = '<ul>
              <li>沈腾</li>
              <li>马丽</li>
              <li>艾伦</li>
              <li>魏翔</li>
          </ul>';
      //2.字符串中进行变量拼接
      let star = '魏翔'
      let str2 ='我特别喜欢${star},搞笑风格诡异';
      console.log(str2);

      //简化对象写法
      let  name = '尚硅谷';
      let  pos = '北京';
      let  change = function(){
        console.log('改变');
      }
      const atguigu = {
      name,
      pos,
      change,
      impro(){
      console,log('提升');
      }
      }
      console.log(atguigu)

      //箭头函数
      //ES6  允许使用箭头(=>)定义函数.
      //1.声明格式
      let add = (a,b,c) =>{
      let result = a + b + c;
      return result;
      };
      //2.函数调用
      console.log(add(1,2,3,));
      console.log(add.call({},1,2,3));
      console.log(add.apply({}, [1,2,3]));

      //箭头函数特点
      //1.this 的值是静态的.
      let  getName2 = () => {
      console.log(this);
      }
      getName2.call({});
      //2不能作为构造函数使用
      const  Person = () => {}
      let  me = new Person();
      //3.不能使用 arguments
      const Person = () => {
      console.log(arguments);
      }
      fn(1,3,5,8,10);
      //4. 箭头函数简写
      //一 不写小括号, 当形参有且只有一个的时候
      //二 不写花括号,当代码只有一条语句的时候,并且语句的执行结果为函数返回值的(如果不写花括号的话,return 也不能写)
      let  pow = num => num*num*num;
      let rand = (m,n) => Math.ceil(Math.random() * (n-m+1)) + m-1;

      console.log(pow(9));
      console.log(rand(1,100));
      //ES6 允许给函数参数赋值初始值
      //1.参数直接设置默认值  具有默认值的参数,位置一般要靠后(潜规则)
      finction add(a,b=10,c){
      console.log( a + b + c );
      }
      //add(1,2,4);
      //add(1,2);
      //2.与解构赋值结合使用  结构赋值的形式先后顺序不影响
      function connect({host="127.0.0.1",port,pass,dbname}){
      console.log(host);
      console.log(port);
      console.log(pass);
      console.log(dbname);
      }
      connect({
      port: 27017,
      pass: 'root',
      dbname: 'project'
      });
      //rest参数
      function main(...args){
      //1.使用arguments 获取实参
      console.log(arguments);
      //2.rest 参数
      console.log(args);
      }
      main(1,5,10,20,25);  // 1,5,10,20,25  => ...[1,5,10,20,25] => 1,5,10,20,25
      //3.如果有多个参数
      function fn(a,b,...args){
      console.log(a);
      console.log(b);
      console.log(args);
      }
      fn(1,2,3,4,5,6,7);

      //扩展运算符应用
      //1.数组的合并
      const kuaizi = ['肖央','王太利'];
      const chuanqi = ['曾毅','玲花'];
      const zuhe = [...kuaizi,...chuanqi];
      console.log(zuhe);
      //2.新数组克隆
      const jinhua =['e','g','m'];
      const s = [...jinhua,'x','y','z'];
      console.log(s);
      //3.将伪数组转为真正的数组
      const divs = document.querySelectorAll('div')
      const result = [...divs];
      console.log(resule);


      //set  声明集合
      const s2 = new Set([1,5,9,13,17,1,5]);
      //1.元素个数
      console.log(s2.size);
      //2.添加
      s2.add(21);
      //3.删除
      s2.delete(5);
      //4.检测集合中是否包含某个元素      has    有
      console.log(32.has(90));
      //5.clear 清空
      s2.clear();
      //6. for...of 遍历
      for(let v of s2){
      console.log(v)
      }


      //对象声明
      //语法
      class phone {
      //构造方法
      constructor(brand, price){
            this.brand = brand;
            this.price   = price;
      }
      //方法
      call(someone){
          console.log('我可以给 ${someone}打电话');
      }
      sendMessage(someone){
          console.log('我可以给 ${someone} 发送短信');
      }
      }
      const  jinli = new phone('金立',599);
      console.log(jinli);
      //1.构造方法不是必须的
      class A {
      //2.构造方法只能有一个
      constructor(){}
      constructor(){}
      }
      let a = new A();
      console.log(a);

      //对象静态属性和方法
      function  phone(brand){
      //为实例对象添加属性
      this.brand = brand;
      }
      //为函数对象添加属性
      phone.name ='手机'; // 又称之为  静态成员 
      phone.change = function(){
              console.log('改变了世界');
      }
      let  nokie = new  phone('诺基亚');
      console.log(nokia);
      console.log(phone);
      //_________________________
      class phone{
      static  静态的
      static  name = '手机';
      static change(){
            console.log('我改变了世界');
      }
      }

      console.log(phone.name);
      phone.change();

      ```