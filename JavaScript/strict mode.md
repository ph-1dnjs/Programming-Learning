# strict mode  
  
```javascript  
function foo() {
	x = 10;
}
foo();

console.log(x); // ?   
```  
  
  
## strict mode란?  
---  
  
  
  
  
  
  
## strict mode의 적용  
---  
  
  
전체 스크립트에 적용  
```javascript  
'use strict';

function foo() {
	x = 10; // ReferenceError: x is not defined
}
foo();  
```  
  
함수에 적용  
```javascript  

function foo() {
	'use strict';
	x = 10; // ReferenceError: x is not defined
}
foo();  
```  
  
잘못된 예  
```javascript  
function foo() {
	x = 10; // 에러를 발생시키지 않는다.
	'use strict';
}
foo();  
```  
  
## 전역에 strict mode를 적용하는 것은 피하자  
---  
```html  
<!DOCTYPE html>
<html>
<body>
	<script>
		'use script';
	</script>
	<script>
		x = 1; // 에러가 발생하지 않는다.
		console.log(x); // 1
	</script>
	<script>
		'use script';
		
		y = 1; // ReferenceError: y is not defined
		console.log(y);
	</script>
</body>
</html>  
```  
스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정되어 적용된다.  
  
### strict mode와 non-strict mode 혼용 시 문제점 및 해결 방법  
📌 문제점  
strict mode와 non-strict mode를 혼용하면 예상치 못한 오류가 발생할 수 있다.   
⚠️ 원인  
* 외부 서드파티 라이브러리가 non-strict mode일 가능성이 있다.  
* strict mode를 전역에 적용하면 non-strict mode 코드와 충돌할 위험이 있다.  
 ✅ 해결 방법  
* 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 분리한다.  
* 함수 내부 선두에 strict mode를 적용한다.  
  
```javascript  
(function () {
	"use strict";  // strict mode 적용

	// 여기에 strict mode 적용 코드 작성
})();  
```  
  
## 함수 단위로 strict mode를 적용하는 것도 피하자  
---  
📌 문제점  
1. strict mode가 함수마다 다르게 적용될 가능성  
1. 모든 함수마다 strict mode를 적용하는 건 번거롭다  
1. 함수의 바깥 컨텍스트가 strict mode가 아닐 수도 있음  
  
```javascript  
(function () {
	// non-strict mode
	var let = 10; // 에러가 발생하지 않는다.
	
	function foo() {
		'use strict';
		
		let = 20; // SyntaxError: Unexpected strict mode reserved word
	}
})();  
```  
  
💡 왜 문제가 되는 걸까?  
* 즉시 실행 함수 바깥은 non-strict mode이므로 var let = 10;이 에러 없이 실행된다.  
* foo 함수 안에서는 strict mode가 적용되므로 let = 20;에서 SyntaxError 발생  
* 즉, 같은 코드에서도 strict mode 적용 여부에 따라 동작이 다를 수 있음 → 혼란 발생!  
  
✅ 따라서 strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.  
  
## strict mode가 발생시키는 에러  
---  
### 암묵적 전역  
  
  
```javascript  
(function () {
	'use strict';
	
	x = 1;
	console.log(x); // ReferenceError: x is not defined
}());  
```  
  
### 변수, 함수, 매개변수의 삭제  
  
  
```javascript  
(function () {
	'use strict';
	
	var x = 1;
	delete x; // SyntaxError: Delete of an unqualified identifier in strict mode.
	
	function foo(a) {
		delete a; // SyntaxError: Delete of an unqualified identifier in strict mode.
	}
	delete foo; // SyntaxError: Delete of an unqualified identifier in strict mode.
}());  
```  
  
### 매개변수 이름의 중복  
  
  
```javascript  
(function () {
	'use strict';
	
	// SyntaxError: Duplicate parameter name not allowed in this context
	function foo(x, x) {
		return x + x;
	}
	console.log(foo(1, 2));
}());  
```  
  
### with 문의 사용  
  
  
  
  
```javascript  
(function () {
	'use strict';
	
	// SyntaxError: Strict mode code may not include a with statement
	with({ x: 1 }) {
		console.log(x);
	}
}());  
```  
  
strict mode 에서는 성능과 가독성이 나빠지는 문제로 with 문을 사용할 수 없게 금지했다.  
  
## strict mode 적용에 의한 변화  
---  
  
### 일반 함수의 this  
  
  
```javascript  
(function () {
	'use strict';
	
	function foo() {
		console.log(this); // undefined
	}
	foo();
	
	function Foo() {
		console.log(this); // Foo
	}
	new Foo();
}());  
```  
  
### arguments 객체  
  
  
```javascript  
(function (a) {
	'use strict';
	// 매개변수에 전달된 인수를 재할당하여 변경
	a = 2;
	
	// 변경된 인수가 arguments 객체에 반영되지 않는다.
	console.log(arguments); // { 0: 1 }
}(1));  
```  
