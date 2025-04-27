## DOM 요소의 기본 동작 조작<br>  
---  
  
### DOM 요소의 기본 동작 중단<br>  
이벤트 객체의 preventDefault 메서드는 DOM 요소의 기본 동작을 중단시킨다.  
  
```javascript  
<!DOCTYPE html>
<html>
<body>
	<a href="https://www.google.com">go</a>
	<input type="checkbox">
	<script>
		document.querySelector('a').onclick = e => {
			// a 요소의 기본 동작을 중단한다.
			e.preventDefault();
		};
		
		document.querySelector('input[type=checkbox]").onclick = e => {
			// checkbox 요소의 기본 동작을 중단한다.
			e.preventDefault();
		};
	</script>
</body>
</html>  
```  
  
### 이벤트 전파 방지<br>  
이벤트 객체의 stopPropagation 메서드는 이벤트 전파를 중지한다.  
* 하위 DOM 요소의 이벤트를 개별적으로 처리하기 위해 이벤트를 중단시킨다.  
  
```javascript  
<!DOCTYPE html>
<html>
<body>
	<div class="container">
		<button class="btn1">Button 1</button>
		<button class="btn2">Button 2</button>
		<button class="btn3">Button 3</button>
	</div> 
	<script>
		// 이벤트 위임. 클릭된 하위 요소의 color를 변경한다.
		document.querySelector('.container').onclick = ({target}) => {
			if (!target.matches('.container > button')) return;
			target.style.color = 'red';
		};
		
		// .btn2 요소는 이벤트를 전파하지 않으므로 상위 요소에서 이벤트를 캐치할 수 없다.
		document.querySelector('.btn2").onclick = e => {
			e.stopPropagation(); // 이벤트 전파 중단
			e.target.style.color = 'blue';
		};
	</script>
</body>
</html>  
```  
* btn2 요소는 자신이 발생시킨 이벤트가 전파되는 것을 중단하여 자신에게 바인딩된 이벤트 핸들러만 실행되도록 한다.  
  
  
## 이벤트 핸들러 내부의 this<br>  
---  
  
### 이벤트 핸들러 어트리뷰트 방식<br>  
```javascript  
<!DOCTYPE>
<html>
<body>
	<button onclick="handleClick(this)">Click me</button>
	<script>
		function handleClick(button) {
			console.log(button); // 이벤트를 바인딩한 button 요소
			console.log(this);   // window
		};
	</script>
</body>
</html>  
```  
* 이벤트 핸들러 어트리뷰트의 값으로 지정한 문자열은 암묵적으로 생성되는 이벤트 핸들러의 문이다. 따라서 handleClick은 이벤트 핸들러에 의해 일반 함수로 호출된다. → 일반 함수로 호출되는 함수 내부의 this는 전역 객체를 가리킨다. (window)  
* 이벤트 핸들러를 호출할 때 인수로 전달한 this는 이벤트를 바인딩한 DOM 요소를 가리킨다.  
  
### 이벤트 핸들러 프로퍼티 방식 & addEventListener 메서드 방식<br>  
이벤트 핸들러 프로퍼티 방식과 addEventListenter 메서드 방식 모두 이벤트 핸들러 내부 this는 이벤트를 바인딩한 DOM 요소를 가리킨다.  
* 이벤트 핸들러 내부의 this는 이벤트 객체의 currentTarget 프로퍼티와 같다.  
  
```javascript  
<!DOCTYPE>
<html>
<body>
	<button class="btn1">0</button>
	<button class="btn2">0</button>
	<script>
		const $button1 = document.querySelector('.btn1');
		const $button2 = document.querySelector('.btn2');
		
		// 이벤트 핸들러 프로퍼티 방식
		$button1.onclick = e => {
			// 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
			console.log(this); // window
			console.log(e.currentTarget); // $button1
			console.log(this === e.currentTarget); // false
		};
		
		// addEventListener 메서드 방식
		$button2.addEventListener('click', e => {
			// 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
			console.log(this); // window
			console.log(e.currentTarget); // $button2
			console.log(this === e.currentTarget); // false
			
			// this는 window를 가리키므로 window.textContent에 NaN(undefined + 1)을 할당한다.
			++this.textContent;
		});
	</script>
</body>
</html>  
```  
* 화살표 함수로 정의한 이벤트 핸들러 내부의 this는 상위 스코프의 this를 가리킨다.  
* 화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다.  
  
## 이벤트 핸들러에 인수 전달<br>  
---  
함수에 이벤트를 전달하려면 함수가 호출할 때 전달해야한다. 이벤트 핸들러 프로퍼티 방식이나 addEventListener 방식의 경우 이벤트 핸들러를 브라우저가 호출하기 때문에 호출문이 아닌 함수 자체를 등록해야 한다. 따라서 인수를 전달할 수 없다.  
  
1. 이벤트 핸들러 내부에서 함수를 호출하며 인수 전달  
1. 이벤트 핸들러를 반환하는 함수를 호출하며 인수를 전달  
## 커스텀 이벤트 <br>  
---  
이벤트 객체는 개발자의 의도로 생성된 이벤트를 커스텀 이벤트라고 한다.  
* 이벤트 생성자 함수는 첫 번째 인수로 이벤트 타입을 나타내는 문자열을 전달받는다. 기존 이벤트 타입 사용 or 임의의 문자열로 새로운 타입 지정 가능  
* 임의의 문자열을 사용하는 경우에는 `CustomEvent` 이벤트 생성자 함수를 사용한다.  
  
### 커스텀 이벤트 생성<br>  
```javascript  
// KeyboardEvent 생성자 함수로 keyup 이벤트 타입의 커스텀 이벤트 객체를 생성
const keyboardEvent = new KeyboardEvent('keyup');
console.log(keyboardEvent.type); // keyup

// CustomEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new CustomEvent('foo');
console.log(customEvent.type); // foo  
```  
* 생성된 커스텀 이벤트 객체는 버블링되지 않으며 preventDefault 메서드로 취소할 수도 없다.  
* 커스텀 이벤트 객체는 bubbles와 cancelable 프로퍼티의 값이 false로 기본 설정된다.  
  
커스텀 이벤트 객체의 bubbles와 cancelable 프로퍼티를 true로 설정하려면 이벤트 생성자 함수의 두 번째 인수로 프로퍼티 값을 갖는 객체를 전달한다.  
```javascript  
// MouseEvent 생성자 함수로 click 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new MouseEvent('click', {
	bubbles: true,
	canclelable: true
});  
```  
  
### 커스텀 이벤트 디스패치<br>  
* 생성된 커스텀 이벤트는 `dispatchEvent` 메서드로 디스패치(이벤트를 발생시키는 행위)할 수 있다.  
* `dispatchEvent` 메서드에 이벤트 객체를 인수로 전달하면서 호출하면 인수로 전달한 이벤트 타입의 이벤트가 발생한다.  
* 기존 타입이 아닌 임의의 이벤트 타입을 지정하여 커스텀 이벤트 객체를 생성한 경우 반드시`addEventListener` 메서드 방식으로 이벤트 핸들러를 등록해야 한다.  
* 이벤트 핸들러 어트리뷰트/프로퍼티 방식을 사용할 수 없는 이유는 `on + 이벤트` 타입으로 이루어진 이벤트 핸들러 어트리뷰트/프로퍼티가 요소 노드에 존재하지 않기 때문이다.  
   * ex) 'foo' 라는 임의의 이벤트 타입으로 커스텀 이벤트를 생성할 경우 'onfoo'라는 핸들러 어트리뷰트/프로퍼티가 요소 노드에 존재하지 않는다.  
  
