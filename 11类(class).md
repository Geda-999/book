# 类(class)

- 简单的类写法

```js
//es5
function Person(name) {
  this.name = name;
}
Person.prototype.age = 18;
new Person();

//es6
class Person {
  constructor(name) {
    this.name = name;
  }
}
new Person();

//ts
class Person{
  constructor(name:string){
    this.name=name
  }
  // name:string    也可以这种样子写
  public name:string
}
let p = new Person("张三")
```

## 1、属性和方法

使用 `class` 定义类，使用 `constructor` 定义构造函数。

通过 `new` 生成新实例的时候，会自动调用构造函数。

```js
class Animal{
    public name
    constructor(name:string){
        this.name = name
    }
    sayHi(){
        return `my name is ${this.name}`
    }
}

let a = new Animal("小明")

console.log(a.name)
console.log(a.sayHi())
```

## 2、类的继承

使用 `extends` 关键字实现继承，子类中使用 `super` 关键字来调用父类的构造函数和方法。

```js
class Person{
    constructor(name:string,age:number){
        this.name = name
        this.age = age
    }
    public name:string
    public age:number
}

class Teacher extends Person{
    constructor(name:string,age:number,sex:string){
        super(name,age)
        this.sex = sex
    }
    public sex:string
    sayHello(){
        console.log(this.name+this.age+this.sex)
    }
}

let t = new Teacher("小明",18,"男")

t.sayHello()//小明18男
```

```js
//类的继承
class Person{
  constructor(name:string){
    this.name=name
  }
  public name:string
  say():void{
    console.log(`我的名字是${this.name}`)
  }
}
class Student export Person{
  constructor(name:string){
    super(name) 
  }
}
let s =new Person("张三")
s.say() //我的名字是张三

```

## 3、存取器

使用 getter 和 setter 可以改变属性的赋值和读取行为：

```js
//使用存取器的时候不加constructor
class Animal{
    get name(){
        return "小明"
    }
    set name(val){
        console.log('setter:'+val)
    }
}
let a = new Animal("小红")//setter:小红

console.log(a.name)//小明
```

```js
//练习
class Person {
  get name() {
    return '张三';
  }
  set name(newName: string) {
    //修改值
    console.log(newName);
  }
}
let p = new Person();
console.log(p.name);
p.name = '李四';
```

## 4、静态属性和静态方法

使用 `static` 修饰符修饰的属性和方法称为静态属性和静态方法，它们不需要实例化，而是直接通过类来调用

```js
class Animal{
    static isAnimal(){
        console.log("静态方法")
    }
    static num = 1
}

Animal.isAnimal()//静态方法
console.log(Animal.num)//1
```

```js
//练习

//静态属性
class Person{
  constructor(name:string){
    this.name=name
  }
  public name:string
  say():void{
    console.log("我的名字是"+this.name)
  }
}
let p =new Person("张三")
p.say() //我的名字是张三

//静态方法
class Person{
  constructor(name:string){
    this.name=name
  }
  public name:string
  say():void{
    console.log("我的名字是"+this.name)
  }
  static age:number=18
  static sayHello():void{
    console.log("hello")
  }
}
let p =new Person("张三")
console.log(Person.age)
Person.sayHello()
```

## 5、修饰符

TypeScript 可以使用三种访问修饰符（Access Modifiers），分别是 `public`、`private` 和 `protected`。

- `public` 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 `public` 的

- `private` 修饰的属性或方法是私有的，不能在声明它的类的外部访问

- `protected` 修饰的属性或方法是受保护的，它和 `private` 类似，区别是它在子类中也是允许被访问的

  ```js
  public：   公有属性 所有地方都可以使用
  private：  私有属性 只有自己可以使用
  protected：受保护属性 只有自己和继承类可以使用
  ```

```js
//public
class Animal{
    public name
    public constructor(name:string){
        this.name = name
    }
}
let a = new Animal("小明")
console.log(a.name)//小明
a.name = "小红"
console.log(a.name)//小红

//private
//虽然ts报错，但是编译后的代码仍然能访问
class Animal{
    private name
    public constructor(name:string){
        this.name = name
    }
}
let a = new Animal("小明")
console.log(a.name)//小明
a.name = "小红"
console.log(a.name)//小红

//子类也无法访问
class Animal{
    private name
    public constructor(name:string){
        this.name = name
    }
}
class Cat extends Animal{
    constructor(name:string){
        super(name)
        console.log(this.name)
    }
}

//protected 受保护属性 只有自己和继承类可以使用
class Animal{
    protected name
    public constructor(name:string){
        this.name = name
    }
}

//子类可以访问，但是实例化不能
class Cat extends Animal{
    constructor(name:string){
        super(name)
        console.log(this.name)//不报错
    }
}

let c = new Cat("tom")
c.name = "jack"//报错
//当构造函数修饰为 private 时，该类不允许被继承或者实例化
class Animal {
  public name;
  private constructor(name) {
    this.name = name;
  }
}
//报错
class Cat extends Animal {
  constructor(name) {
    super(name);
  }
}

let a = new Animal('Jack');//报错
//当构造函数修饰为 protected 时，该类只允许被继承
class Animal {
  public name;
  protected constructor(name) {
    this.name = name;
  }
}
//不报错
class Cat extends Animal {
  constructor(name) {
    super(name);
  }
}

let a = new Animal('Jack');//报错
```

## 6、参数属性

```js
//修饰符和readonly还可以使用在构造函数参数中，
//等同于类中定义该属性同时给该属性赋值，使代码更简洁。
class Animal{
    // public name
    public constructor(public name:string){
        this.name = name
    }
}
let a = new Animal("小明")
console.log(a.name)
```

## 7、只读属性readonly

```js
//只读属性关键字，只允许出现在属性声明或索引签名或构造函数中。
class Animal {
  readonly name;
  public constructor(name) {
    this.name = name;
  }
}

let a = new Animal('Jack');
console.log(a.name); // Jack
a.name = 'Tom';//不能修改

//注意如果 readonly 和其他访问修饰符同时存在的话，需要写在其后面。
class Animal {
  public constructor(public readonly name) {

  }
}
```

## 8、抽象类

`abstract` 用于定义抽象类和其中的抽象方法。

抽象类的意思是不允许被实例化，抽象类中的抽象方法必须被子类实现。

```js
abstract class Animal{
    public constructor(public name:string) {
        this.name = name
    }
}

let a = new Animal("cat")//报错，无法实现实例化
//子类没有实现父类的抽象方法，所以报错
abstract class Animal{
    public constructor(public name:string) {
        this.name = name
    }
    public abstract sayHello():void
}

class Cat extends Animal{

}
let c = new Cat("tom")

//正确
abstract class Animal{
    public constructor(public name:string) {
        this.name = name
    }
    public abstract sayHello():void
}

class Cat extends Animal{
    public sayHello(){
        console.log("hello")
    }
}

let c = new Cat("tom")
```

## 9、类可以作为接口使用

```js
class Animal{
    public name:string = "小明"
    public age:number = 18
    public sex:string = "男"
}

let a:Animal = {
    name:"小红",
    age:19,
    sex:"女"
}

console.log(a)
```

```js
//真正写接口法
interface Obj{
  name:string;
  age:number;
  sex:string;
}

let obj:Obj={
  name:"张三",
  age:18,
  sex:"男"
}
```

