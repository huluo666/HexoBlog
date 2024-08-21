---
title: js创建对象和方法
tags:categories: JavaScript
date: 2018-01-03 17:57:32
grammar_cjkRuby: true
---

一.直接创建

通过键值对的形式将对象中的属性和方法连接起来

```javascript
var person = {
    name : '张三',
    age : 18,
    sayHi : function () {
        alert('hello');
    }
}
```

1、先创建对象，然后添加属性和方法

```javascript
数组字面量法
<script>
    var hotel={}
    hotel.name='Quay';
    hotel.rooms=40;
    hotel.booked=25;
    hotel.checkAvilablity=function(){
        return this.rooms-this.booked
    }
alert(hotel.name)
</script>


对象构造函数法
<script>
    var hotel=new Object()
    hotel.name='Quay';
    hotel.rooms=40;
    hotel.booked=25;
    hotel.checkAvilablity=function(){
        return this.rooms-this.booked
    }
    alert(hotel.name)
</script>
```

2、创建对象的同时创建属性和方法

```
字面量法
<script>
    var hotel={
        name:'Quay',
        rooms:40,
        booked:25,
        checkAvilablity:function(){
        return this.rooms-this.booked
    }
    }
alert(hotel.name)
</script>

构造函数法
<script>
function Hotel(name,rooms,booked){
    this.name=name;
    this.rooms=rooms;
    this.booked=booked;
    this.checkAvilablity=function(){
        return this.rooms-this.booked;
    }
}

var quayhotel=new Hotel('Quay',40,25);
alert(quayhotel.name);
var parkhotel =new Hotel('Park',120,77);
alert(parkhotel.name);
</script>
```



二.使用工厂模式创建对象

```javascript
function createPerson (name, age) {
    // 创建一个空对象
    var per = new Object();
    per.name = name;
    per.age = age;
    per.sayHi = function () {
        alert('hello');
    }
    // 把创建好的对象返回出去
    return per;
}
var person1 = createPerson('张三', 18)
var person2 = createPerson('李四', 20)
```

通过创建一个空对象，在空对象中加入相关属性和属性值，最后记得返回出创建的对象



三.使用构造函数创建对象

```javascript
function CreatePerson (name, age) {
   this.name = name;
   this.age = age;
   this.sayHi = function () {
       alert('hello');
   }
}
var person = new CreatePerson('张三', 18);
```



四.通过原型创建对象

```javascript
function CreatePerson (name, age) {
    this.name  = name;
    this.age  = age;
    CreatePerson.prototype.sayHi  = function () {
        alert('hello');
    };
}
```

此方法就是为了改进对于相同的属性

https://www.jianshu.com/p/01f90948cfe7

创建函数

```javascript
function myFunction(a,b){
	return a+b;
}

//等价于
var myFunction=new Function("a","b","return a+b");
```

