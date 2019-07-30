# JavaScript继承

##### 先提供一个父类

```javascript
//父类
function Person(name) {
	this.name = name;
	this.sum = function(){
		alert(this.name);
	}
}
Person.prototype.age = 10; //给构造函数添加了原型属性 
```

### 1.原型链继承

```javascript
function Per(){
	this.name = "ker";
}
Per.prototype = new Person();
var per1 = new Per();
//instanceof 判断元素是否在另一个元素的原型链上
//per1 继承了Person的属性 返回true
console.log(per1 instanceof Person);//true
```

* 重点: 让新实例的原型等于父类的实例

* 特点:
  * 实例可继承的属性有: 实例的构造函数的属性
  * 父类构造函数属性
  * 父类原型的属性
  
* 缺点: 

  * 新实例无法向父类构造函数传参

  * 继承单一

  * 所有新实例都会共享父类实例的属性


### 2.使用构造函数继承

```javascript
function Con(){
	Person.call(this,"jer");
	this.age = 12;
}
var con1 = new Con();
console.log(con1.name);
console.log(con1.age);
console.log(con1 instanceof Person);//false
```

* 重点: 用.call()或.apply() 将父类构造函数引入子类函数

* 特点:

  * 只继承了父类构造函数的属性，没有继承父类原型的属性
  * 解决了原型链继承缺点
  * 可以继承多个构造函数属性(call 多个)
  * 在子实例中可向父实例传参

* 缺点
  * 只能继承父类构造函数的属性
  * 无法实现构造函数的复用 (每次用每次都有重新调用)

### 3.组合继承(组合原型链继承和借用构造函数继承)

```javascript
function SubType(name) {
	Person.call(this,name);//构造函数继承
}
SubType.prototype = new Person();//原型链继承
var sub = new SubType("ger");
console.log(sub.name);
console.log(sub.age);
```

* 重点: 结合了两种模式的优点，传参和复用
* 特点
  * 可以继承父类原型上的属性，可以传参，可复用
  * 每个新实例引入的构造函数属性是私有的
* 缺点：
  * 调用了两次父类构造函数（耗内存），子类的构造函数会代替原型上的构造函数



## 4.寄生组合式继承

```javascript
//寄生
function content(obj){
	function F(){}
	F.prototype = obj;
	return new F();
}
var con = content(Person.prototype);
//con实例（F实例）的原型继承了父类函数的原型
//上述就像原型链继承，只不过只继承了原型属性

//组合
function Sub(){
	Person.call(this);
}//继承了父类构造函数的属性解决了组合式两次调用构造函数属性的缺点

Sub.prototype = con;
con.constructor = Sub;//修复实例
var sub1 = new Sub();
//Sub实例就继承了构造函数属性，父类实例，con的函数属性

----------------------------------------------------------------------------------
//不同写法
function F(){};
F.prototype = Person.prototype;
function Sub(){
	Person.call(this);
}
Sub.prototype = new F();
var sub2 = new Sub();
```

* 重点:修复了组合继承的问题

