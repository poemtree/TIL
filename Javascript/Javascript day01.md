# Javascript

## 자료형

1. 자료형

   1. 숫자
   2. 문자
   3. null => 아무것도 없다는 값으로 들어있음
   4. undefined => 값이 할당되어 있지 않음
   5. 논리(boolean) => true, false
   6. 객체(Object) => { } // hash
      1. 키와 값을 쌍으로 가짐
         1. var o = {key: value}
      2. 함수를 값으로 가질 수 있음
      3. 속성.. method? property?
         1. value에 함수가 들어있을 경우, method
         2. 그 외의 값으로 된 속성은 property
   7. 배열(array) => [ ]
      1. 유용한 methods
         1. pop() = 맨 뒤에 값을 빼낸다(스택)
         2. shift() = 맨 앞에 값을 빼낸다(큐)
         3. unshift(Element) = 맨 앞에 Element를 넣는다
         4. push(Element) = 맨 뒤에 Element를 넣는다
         5. sort() = 정렬
         6. reverse() = 역순 정렬
         7. indexOf(Element) = 배열에서 Element의 위치 반환(0부터 시작)
         8. forEach(function(Element)) = 배열의 요소 하나하나를 인자로 function()을 실행
         9. map(function(Element)) = forEach의 결과를 배열로 반환 -> 즉, function()에 return이 있어야 한다.
         10. filter(function(Element)) = 배열의 요소 하나하나를 인자로 function()을 실행한 후  값이 true인 인자들 만 반환 -> 즉, function()의 return은 boolean이어야 한다.
         11. reduce(function(Element1, Element2)) = 배열의 요소 처음 두 개를 인자로 function()을 실행한 후 그 결과와 배열의 다음 요소를 인자로 수행하기를 반복한다.

2. 함수

   1. 함수

      ```javascript
      function sum(x,y) {
          console.log(x+y);
      }
      ```

   2. 함수 표현식

      ```javascript
      var sum = function(x,y) {
          console.log(x+y);
      }
      ```

   3. 차이점

      ```javascript
      // 실제 작성 코드1:
      sum(1,2);
      function sum(x,y) {
          console.log(x+y);
      }
      /* 위의 코드가 실행되는 순서
      function sum(x,y) {
          console.log(x+y);
      }
      sum(1,2);
      */
      ```

      ```javascript
      // 실제 작성 코드2:
      sum(1,2);
      var sum = function(x,y) {
          console.log(x+y);
      }
      /* 위의 코드가 실행되는 순서
      var sum;
      sum(1,2);
      sum = function(x,y) {
          console.log(x+y);
      }
      */
      ```

      **Hoisting : Javasctipt는 변수와 함수의 선언문을 선행한다.**

   4. 함수..

      ```javascript
      var arr = [1,2,3,4,5];
      var double = function(x) {return x*2};
      //var arr2 = arr.map(double)
      
      var arr2 = map(double, arr)
      function map(func, arr){
          var new_arr = [];
          for (element of arr){
              new_arr.push(func(element));
          }
          return new_arr;
      }
      
      arr2 //[2, 4, 6, 8, 10]
      
      var positive = function(x){return x>0};
      //positive(1) => true
      //positive(-1) => false
      var arr = [-1, 3, -5, 7, -9]
      //var arr2 =  arr.filter(positive)
      
      var arr2 = filter(positive, arr) 
      function filter(func, arr){
          var new_arr = [];
           for (element of arr){
              if(func(element)){
                 new_arr.push(element);
              }
          }
          return new_arr;
      }
      
      arr2 //[3,7]
      
      //2. 함수리턴
      function func1() {
          return function func2() {
              console.log("I'm inner function")
          }
      }
      var test = func1();
      ```

   5. 함수의  인자 사용법

      ```javascript
      function sum1(a,b) {
          console.log(a+b);
      }
      sum1(1) // => 들어오지 않은 값은 undefined
      // console.log(1+undefined);
      // => NaN
      sum1(1,2,3) // => 더 들어오는 인자도 arguments로 사용 가능
      
      function sum2(a, b) {
          var total = 0;
          for (element of arguments) {
              total += element;
          }
          return total;
      }
      
      sum2(1,2,3,4,5) // => 15
      
      function multi() {
          var result = 1;
          for (element of arguments) {
              result *= element;
          }
          return result;
      }
      
      multi(1,2,3) // => 6
      ```

3. 변수 스코프

   ```javascript
   // 전역 변수
   var i = 0;
   function changeI() {
       i = 10;
       console.log(i);
   }
   
   changeI();		//10
   console.log(i)	//10
   
   // 지역 변수
   var j = 0;
   function changeJ() {
       var j = 10;
       console.log(j);
   }
   
   changeJ();		//10
   console.log(j)	//10
   ```

   ```javascript
   // 정적 유효범위
   var i = 0;
   
   function a() {
       var i = 10;
       b(); // 함수 호출 시 변수 참조 x
   }
   
   function b() {
       console.log(i) // 함수 선언 시 변수 참조
       // 함수가 선언될 당시 i는 0...
   }
   
   a() //콘솔에 출력되는 값은?? => 0
   
   i = 10;
   a() //콘솔에 출력되는 값은?? => 10
   
   var j = 0;
   
   function c() {
       var j = 10;
       function d() {
           console.log(j);
       }
       b();
   }
   
   c() // 콘솔에 출력되는 값은?? =? 10
   
   //클로저
   var z = 0;
   function e() {
       var z = 10;
       return function f(){
           console.log(z);
       }
   }
   var closure = e();
   closure() // 콘솔에 출력되는 값은?? => 10
   ```

4. Hoisting(끌어올림)

   ```javascript
   // 예시1.
   // 변수 선언 전에 참조하도록 작성
   console.log(i);
   var i = 0;
   
   // 함수 선언 전에 호출하도록 작성
   func();
   function func() {
       console.log("func!!");
   }
   
   // => undefined
   // => func!!
   ```

   ```javascript
   // 예시1. 코드가 실행될 때
   // 함수, 변수의 선언문이 먼저 실행
   function func() {
       console.log("func!!");
   }
   var i;
   
   console.log(i); // 선언만 되어있는 빈 변수 참조
   i = 0;	// 변수에 값 대입
   
   func(); // 함수 호출
   
   // => undefined
   // => func!!
   ```

   ```javascript
   // 예시3.
   var language = 'Java';
   
   function checkScript(script) {
       if (script) {
           var language = "ruby";
           console.log(language);
       } else {
           console.log(language);
       }
   }
   
   checkScript(true); // 콘솔에 출력되는 값은?? => ruby
   checkScript(false); // 콘솔에 출력되는 값은?? => undefined
   ```

   ```javascript
   // 예시3. 코드가 실행될 때
   var language
   language = 'Java';
   
   function checkScript(script) {
       var language;
       if (script) {
           language = "ruby";
           console.log(language);
       } else {
           console.log(language);	// 함수 안에서 다시 변수가 선언되어 빈 변수를 참조
       }
   }
   
   checkScript(true);
   checkScript(false); 
   
   // => ruby
   // => undefined
   ```

   ```javascript
   // 예시3. 해결책1
   // 전역 변수만 사용
   var language = 'Java';
   
   function checkScript(script) {
       if (script) {
           language = "ruby"; // 전역 변수에 값을 넣는다.
           console.log(language);
       } else {
           console.log(language);
       }
   }
   ```

   ```javascript
   // 예시3 해결책2
   // let으로 지역 변수를 선언한다.
   var language = 'Java';
   
   function checkScript(script) {
       if (script) {
           let = language = "ruby"; // let으로 선언된 변수는 { } 안에서만 유효하다
           console.log(language);
       } else {
           console.log(language);
       }
   }
   ```

5. this

   ```javascript
   var globalThis = null;
   // 예시1. 함수에서 사용되는 this
   function this1() {
       globalThis = this; // window -> 현재 코드가 실행되는 브라우저의 창을 반환
   }
   this1();
   globalThis;
   
   // 예시2. method에서 사용되는 this -> this는 메소드를 사용하는 객체
   var o = {
       p1: 'property1',
       m1: this1
   };
   
   o.p1
   o.m1()
   // 예시2-1.
   var o2 = {
       prop1: 1,
       method: function() {
           console.log(this.prop1);
       }
   }
   
   o2.method(); // 콘솔에 출력되는 값은? 1 => this.prop1 == o2.prop1
   
   // 예시3. 생성자에서 사용되는 this
   function Person(name) {
       this.name = name;
   }
   
   var p1 = new Person("Poemtree")
   p1 // => Person {name: "Poemtree"}
   
   // 예시3-1.
   var globalThis = null;
   function this1() {
       globalThis = this;
   }
   
   var o1 = new this1();
   
   globalThis
   ```

6. this와 관련된 methods(call, apply, bind)

   ```javascript
   // call()은 function() 안에 this를 call()의 인자로 바꿔준다
   // 만약 function()이 인자를 받는다면 call()의 두 번째 인자부터 들어간다.(첫 번째는 this이기 때문에..)
   // call() 예시1.
   function Person(name) {
       this.name = name;
   }
   var p1 = new Person("Joseph");
   var p2 = {};
   Person.call(p2, "Poemtree"); // call(this, arguments)
   p1 // => Person {name: "Joseph"}
   p2 // => {name: "Poemtree"}
   
   // call 예시2.
   function Person(name, age) {
       this.name = name;
       this.age = age;
   }
   var p3 ={};
   Person.call(p3, "Poemtree", 30)
   p3 // {name: "Poemtree", age: 30}
   
   // call 예시3.
   var globalThis = null;
   function testFunc() {
       globalThis = this;
   }
   var testVar = 20;
   testFunc.call(testVar);
   ```

   ```javascript
   // apply()는 call()과 동일하지만 arguments를 넣는 방식이 다르다.
   // apply(this에 해당하는 대상, [argument1, argument2, ...])
   // apply 예시1.
   function Person(name, age) {
       this.name = name;
       this.age = age;
   }
   var p1 = Person.call(p2, "Poemtree", 30);
   var p2 = Person.call(p2, ["Poemtree", 30]);
   // p1과 p2는 같은 값을 갖는다
   ```

   ```javascript
   // bind()는 function()에 this를 지정한다.. 말 그대로 bind!
   // bind 예시1.
   var globalThis = null;
   
   function testFucn() {
       globalThis = this;
       console.log(globalThis);
   }
   
   var bindedFucn = testFucn.bind(20); // this가 20으로 지정된 testFucn()과 동일한 함수를 반환
   
   testFucn(); // this는 20이 아니다.. 
   globalThis; // => Window
   
   bindedFucn(); // this는 20..
   globalThis; // => Number {20}
   ```

7. Closure(외부 함수의 변수들에 접근 가능한 내부 함수)

   ```javascript
   // Closure 예시1.
   var i = 0;
   function outer() {
       var i = 10;
       var j = 20;
       var k = 30;
       function inner() {
           var innerVar = 100;
           console.log(i); //outer의 i를 참조
           console.log(j); //outer의 j를 참조
           console.log(k); //outer의 k를 참조
       }
       // console.log(innerVar); // 접근할 수 없어서 오류 발생
       return inner;
   }
   
   var closure = outer();
   
   closure(); // => 10, 20, 30
   
   // Closure 예시2.
   // 배열에 반복문을 사용하여 함수를 넣는다
   var arr=[];
   for(var i=0; i<10; i++) {
       arr.push(function() {
           return i * 20;
       });
   }
   // 출력
   for(j in arr) {
       console.log(arr[j]());
   }
   // => 10 200 -> 200이 10번 출력 됨..
   
   // Closure를 활용하여 해결
   var arr=[];
   for(var i=0; i<10; i++) {
       arr.push(function(i){
           return function() {
               return i * 20;
           }
       }(i));
   }
   
   // let을 활용하여 해결..
   var arr=[];
   for(let i=0; i<10; i++) {
       arr.push(function() {
           return i * 20;
       });
   }
   
   // bind를 활용하여 해결..
   var arr=[];
   for(var i=0; i<10; i++) {
       arr.push(function() {
           return this * 20;
       }.bind(i));
   }
   ```

8. Prototype(상속, 클래스 변수를 정의하는 것과 같은 역활)

   ```javascript
   function Person() {
       this.purpose = "happiness";
   }
   function Adult() {
       this.age = "higher than 20";
   }
   function Child() {
       this.age = "lower than 19"
   }
   Adult.prototype = new Person();
   Child.prototype = new Person();
   var a1 = new Adult();
   a1.purpose;
   
   var c1 = new Child();
   c1.purpose;
   
   var p1 = new Person();
   var p2 = new Person();
   
   Person.prototype.name = "Joseph";
   
   // 1. prototype을 통해 상속을 구현할 수 있다.
   Child.prototype = new Person();
   var c1 = new Child();
   var p1 = new Person();
   c1.purpose == p1.purpose;
   
   // 2. prototype을 통해 속성을 객체 간에 공유할 수 있다.
   Child.prototype.name = "Joseph";
   var c1 = new Child();
   var c2 = new Child();
   c1.name == c2.name
   ```