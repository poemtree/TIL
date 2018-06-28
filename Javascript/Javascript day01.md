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
         1. method? property?
            1. value에 함수가 들어있을 경우, method
            2. 그 외의 값은 property
   7. 배열(array) => [ ]
      1. 유용한 methods
         1. pop() = 맨 뒤에 값을 빼낸다(스택)
         2. shift() = 맨 앞에 값을 빼낸다(큐)
         3. unshift(Object) = 맨 앞에 Object를 넣는다
         4. push(Object) = 맨 뒤에 Object를 넣는다
         5. sort() = 정렬
         6. reverse() = 역순 정렬
         7. indexOf(Object) = 배열에서 Object의 위치 반환(0부터 시작)
         8. forEach(function(Element)) = 배열의 요소 하나하나를 인자로 function()을 실행
         9. map(function(Element)) = forEach의 결과를 배열로 반환 -> 즉, function()에 return이 있어야 한다.
         10. filter(function(Element)) = 배열의 요소 하나하나를 인자로 function()을 실행한 후  값이 true인 인자들 만 반환 -> 즉, function()의 return은 boolean이어야 한다.
         11. reduce(function(Element1, Element2)) = 배열의 요소 처음 두 개를 인자로 function()을 실행한 후 그 결과와 배열의 다음 요소를 인자로 수행하기를 반복한다.

2. 함수

   1. 함수?

   2. 함수 표현식

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

      **Javasctipt는 변수와 함수의 선언문을 코드 맨 위로 올리는 작업을 선행한다.**

