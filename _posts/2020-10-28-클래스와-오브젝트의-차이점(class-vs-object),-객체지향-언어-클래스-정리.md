---
title: 클래스와 오브젝트의 차이점(class vs object), 객체지향 언어 클래스 정리
categories: [Javascript]
tags: [class, object]
---

자바스크립트에서의 class와 object를 알아보자

## 클래스

자바스크립트에서 클래스는 ES 6에서 소개됐다. ES 6 전에는 함수를 이용해서 템플릿을 만들어 클래스를 대체했다. 

기존에 있던 프로토타입을 기반으로 간편하게 쓸 수 있도록 문법만 추가됐다.



#### 클래스 정의

자바의 클래스와 동일하게 생성자와 속성, 메서드로 구성할 수 있다.

```javascript
// 1. Class declarations
class Person {
  // constructor
  constructor(name, age) {
    // fields
    this.name = name;
    this.age = age;
  }
    
  // methods
  speak() {
    console.log(`${this.name}: hello!`);
  }
}

const yghee = new Person('yghee', 20);
console.log(yghee.name);  // yghee
console.log(yghee.age);  // 20
yghee.speak();  // yghee: hello!

```



#### Getter / setters

Getter / setters 메서드는 오브젝트의 정보를 은닉할 수 있게 도와주는 메서드다. 

getter / setters가 있을 때에는 클래스 속성을 저장할 때 getter / setters를 참조하므로 이 메서드들의 반환값의 변수는 속성의 이름과 다르게 네이밍해야한다.

```javascript
// 2. Getter and setters
class User {
  constructor(firstName, lastName, age) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
  }

  get age() {
    return this._age;
  }

  set age(value) {
  // if(value < 0) {
  //     throw Error('age can not be negative');
  // }
  this._age = value < 0 ? 0 : value;
  }
}

const user1 = new User('Steve', 'Job', -1);
console.log(user1.age);  // 0

```



#### Fields (public, private)

public과 private는 자바스크립트에서는 최근에 추가됐다. 기본적으로 많이들 알고 있는 개념이지만 자바스크립트 상에서 쓰지 않는 개발자가 많다.

```javascript
// 3. Fields (public, private)
// Too soon!
class Experiment {
  publicField = 2;
  #privateField = 0;
}
const experiment = new Experiment();
console.log(experiment.publicField);  // 2
console.log(experiment.privateField);  // undefined


```



#### Static properties and methods

static 또한 자바스크립트에 최근에 추가됐다. 속성과 메서드에 static이 선언되면 객체들의 수와는 상관 없이 자체적으로 갖고 있는 속성과 메서드다.

```javascript
// 4. Static properties and methods
// Too soon!
class Article {
  static publisher = 'Yghee Coding';
  constructor(articleNumber) {
    this.articleNumber = articleNumber;
  }

  static printPublisher() {
    console.log(Article.publisher);
  }
}

const article1 = new Article(1);
const article2 = new Article(2);
console.log(Article.publisher);  // Yghee Coding
Article.printPublisher();  // Yghee Coding


```



#### Inheritance

```javascript
// 5. Inheritance
// a way for one class to extend another class.
class Shape {
  constructor(width, height, color) {
    this.width = width;
    this.height = height;
    this.color = color;
  }

  draw() {
    console.log(`drawing ${this.color} color!`);
  }

  getArea() {
    return this.width * this.height;
  }
}

class Rectangle extends Shape {}
class Triangle extends Shape {
  draw() {
    super.draw();
    console.log('ㅅ');
  }
  getArea() {
    return (this.width * this.height) / 2;
  }

  toString() {
    return `Triangle: color: ${this.color}`;
  }
}

const rectangle = new Rectangle(20, 20, 'blue');
rectangle.draw();  // drawing blue color
console.log(rectangle.getArea());  // 400
const triangle = new Triangle(20, 20, 'red');
triangle.draw();  // drawing red color ㅅ
console.log(triangle.getArea());  // 200


```





#### Class checking: instanceof

instanceof는 생성한 객체가 클래스의 인스턴스인지 확인하는 방법이다. 그리고 모든 클래스는 Object를 상속받는다.

```javascript
// 6. Class checking: instanceOf
console.log(rectangle instanceof Rectangle);  // true
console.log(triangle instanceof Rectangle);  // false
console.log(rectangle instanceof Triangle);  // false
console.log(triangle instanceof Shape);  // true
console.log(triangle instanceof Object);  // true
console.log(triangle.toString());  // Triangle: color: red

```

