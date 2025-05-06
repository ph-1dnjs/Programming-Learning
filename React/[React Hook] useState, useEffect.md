# 리액트 훅 깊게 살펴보기<br>  
  
`React Hook` 이란, 리액트 **v16.8**에 새로 도입된 기능으로, 함수형 컴포넌트가 클래스형 컴포넌트의 기능을 사용 할 수 있도록 해주며 대표적인 예로는 useState, useEffect 등이 존재한다.  
  
## 리액트의 모든 훅 파헤치기<br>  
---  
* 클래스 컴포넌트보다 간결하게 작성할 수 있다.  
  
### useState<br>  
> 함수 컴포넌트 내부에서 상태를 정의하고, 이 상태를 관리할 수 있게 해주는 훅이다.  
  
  
**useState 구현 살펴보기**  
```javascript  
import { useState } from 'react';

const [state, setState] = useState(initialState);  
```  
* `useState`의 인수로는 `state`의 초깃값을 넘겨준다.  
   * 아무런 값을 넘겨주지 않으면 초기값은 `undefined`이다.  
* `useState` 훅의 반환 값은 배열이다.  
   * 첫 번째 원소로 `state`값 자체를 사용할 수 있다.  
   * 두 번째 원소 `setState` 함수를 사용해 해당 `state`의 값을 변경할 수 있다.  
  
  
  
**useState 내부의 모습을 구현한 모습**  
```javascript  
const myReact = (function () {
	const global = {};
	let index = 0;
	
	function useState(initialState) {
		if (!global.states) {
			// 애플리케이션 전체의 states 배열을 초기화한다.
			// 최초 접근이라면 빈 배열로 초기화한다.
			global.states = []
		}
	}
	
	// states 정보를 조회해서 현재 상태값이 있는지 확인하고,
	// 없다면 초깃값으로 설정한다.
	const currentState = global.states[index] || initialState
	// states의 값을 위에서 조회한 현재 값으로 업데이트한다.
	global.states[index] = currentState
	
	// 즉시 실행 함수로 setter를 만든다.
	const setState = (function() {
		// 현재 index를 클로저로 가둬나서 이후에도 계속해서 동일한 index에
		// 접근할 수 있도록 한다.
		let currentIndex = index;
		return function (value) {
			global.states[currentIndex] = value
			// (컴포넌트를 렌더링한다. 실제로 컴포넌트를 렌더링하는 코드 생략)
		}
	})();
	
	// useState를 쓸 때마다 index를 하나씩 추가한다. 이 index는 setState에서 사용된다.
	// 즉, 하나의 state마다 index가 할당돼 있어 그 index가 배열의 값(global.states)을
	// 가리키고 필요할 때마다 그 값을 가져오게 한다.
	index = index + 1

	return [currentState, setState];
});  
```  
* 실제 리액트 코드에서는 `useReducer`를 이용해 구현돼 있다.   
* `useReducer`와 `useState`는 크게 다르지 않다. (이후 내용 참고)  
  
  
  
  
  
**게으른 초기화**  
리액트 공식 문서에서 게으른 초기화는 `useState`의 **초깃값이 복잡하거나 무거운 연산을 포함하고 있을 때 사용**하라고 되어있다.   
```javascript  
// 일반적인 useState 사용
// 바로 값을 집어넣는다.
const [count, setCount] = useState(
	Number.parseInt(window.localStorage.getItem(cacheKey)),
);

// 게으른 초기화
// 위 코드와의 차이점은 함수를 실행해 값을 반환한다는 것이다.
const [count, setCount] = useState(() => 
	Number.parseInt(window.localStorage.getItem(cacheKey)),
)  
```  
* `state`가 처음 만들어질 때만 사용된다.  
* 이후 리렌더링이 발생하면 이 함수의 실행은 무시된다.  
   * 리액트에서 랜더링이 실행될 때마다 함수 컴포넌트의 함수가 다시 실행된다.  
   * `useState`의 값도 재실행되어 많은 비용을 요구하는 작업이 들어가 있다면 이는 계속 실행될 위험이 존재한다.  
  
### useEffect<br>  
> useEffect는 애플리케이션 내 컴포넌트의 여러 값들을 활용해 동기적으로 부수 효과를 만드는 매커니즘이다. 그리고 이 부수 효과가 ‘언제’ 일어나는지보다 어떤 상태값과 함께 실행되는지 살펴보는게 중요하다.  
  
  
**useEffect란?**  
```javascript  
function Component() {
	// ...
	useEffect(() => {
		// do something
	}, [props, state]);
	// ...
}  
```  
* 첫 번째 인수로는 실행할 `부수효과가 포함된 함수`를 전달한다.  
* 두 번째 인수로는 `의존성 배열`을 전달한다.  
   * 어느 정도 길이를 가진 배열일 수도, 빈 배열일 수도, 배열 자체를 넣지 않고 생략할 수도 있다.  
  
  
  
**클린업 함수의 목적**  
> 함수 컴포넌트가 리렌더링됐을 때 의존성 변화가 있었을 당시 이전의 값을 기준으로 실행되는, 말 그대로 이전 상태를 청소해 준다.  
  
  
```javascript  
import React, { useEffect, useState } from 'react'

export default function App() {
  const [counter, setCounter] = useState(0)

  function handleClick() {
    setCounter((prev) => prev + 1)
  }

  useEffect(() => {
    function addMouseEvent() {
      console.log(counter)
    }
    
    window.addEventListener('click', addMouseEvent)
    
    // 클린업 함수
    return () => {
      console.log('클린업 함수 실행', counter)
      window.removeEventListener('click', addMouseEvent)
    }
  }, [counter])

  return (
    <>
      <h1>{counter}</h1>
      <button onClick={handleClick}>+</button>
    </>
  )
}  
```  
  
**<실행 결과>**  
```javascript  
클린업 함수 실행 0
1
클린업 함수 실행 1
2
클린업 함수 실행 2
3
클린업 함수 실행 3
4

// ...  
```  
  
클린업 함수는 새로운 값과 렌더링이 일어난 후 실행된다. 클린업 함수는 새로운 값을 기반으로 렌더링 된 이후 실행 되긴 하지만 **변경된 값이 아니라 함수가 정의된 당시 선언된 이전 값을 본다**는 것에 주의해야한다.  
  
`useEffect`는 콜백이 실행될 때마다 이전의 클린업 함수가 존재한다면 그 클린업 함수를 먼저 실행하고 콜백을 실행하게 된다. 따라서 이벤트를 추가하기 전에 **이전에 등록했던 이벤트 핸들러를 삭제하는 코드를 클린업 함수에 추가하는 것이다. **  
  
이렇게 함으로써 특정 이벤트 핸들러가 무한히 추가되는 것을 방지할 수 있다.  
  
**의존성 배열**  
> 의존성 배열은 빈 배열, 아무것도 없는 값, 사용자가 원하는 값을 넣을 수 있다.  
  
  
```javascript  
useEffect(() => {
	console.log('렌더링 됨');
}, []);  
```  
만약 빈 배열을 둔다면 비교할 의존성이 없다고 판단해 최초 렌더링 직후에 실행된 다음부터는 더 이상 실행되지 않는다.  
  
```javascript  
useEffect(() => {
	console.log('렌더링 됨');
});  
```  
아무런 값도 넘겨주지 않는 다면 렌더링이 발생할 때마다 실행된다. 컴포넌트가 렌더링 됐는지 확인하기 위한 방법으로 사용한다.  
  
의존성 배열이 없는 useEffect가 매 렌더링마다 실행된다면 그냥 useEffect 없이 써도 되는 것 아닐까?  
```javascript  
function Component(){
	console.log('렌더링 됨')
}

function Component(){
	useEffect(() => {
    	console.log('렌더링 됨')
	})
}  
```  
  
1. **직접 실행**  
   1. 컴포넌트가 렌더링되는 도중에 실행된다.  
   1. 서버 사이드 렌더링에서 서버에서도 실행된다.  
   1. 함수 컴포넌트의 반환을 지연시키고 무거운 작업에서 렌더링 방해로 성능에 악영향을 미친다.  
1. **useEffect**  
   1. 클라이언트 사이드에서 실행되는 것을 보장해준다.  
   1. useEffect 내부에서는 window객체의 접근에 의존하는 코드 사용이 가능하다.  
   1. 컴포넌트가 렌더링된 후에 어떠한 부수 효과를 일으키고 싶을 때 사용하는 훅이다.  
  
**useEffect의 구현**  
```javascript  
const MyReact = (function () {
	const global = {};
	let index = 0;
	
	function useEffect(callback, dependencies) {
		const hooks = global.hooks;
		
		// 이전 훅 정보가 있는지 확인한다.
		let previousDependencies = hooks[index];
		
		// 변경됐는지 확인
		// 이전 값이 있다면 이전 값을 얕은 비교로 비교해 변경이 일어났는지 확인한다.
		// 이전 값이 없다면 최초 실행이므로 변경이 일어난 것으로 간주해 실행을 유도한다.
		let isDependenciesChanged = previousDependencies
			? dependencies.some(
					(value, idx) => !Object.is(value, previousDependencies[idx]),
				)
			: true;
			
		// 변경이 일어났다면 첫 번째 인수인 콜백 함수를 실행한다.
		if (isDependenciesChanged) {
			callback();
				
			// 다음 훅이 일어날 때를 대비하기 위해 index를 추가한다.
			index++;
			
			// 현재 의존성을 훅에 다시 저장한다.
			hooks[index] = dependencies;
		}
		
		return { useEffect };
	}

})();  
```  
  
* 핵심은 의존성 배열의 이전 값과 현재 값은 얕은 비교다.  
* 리액트는 값을 비교할 때 Object.is를  기반으로 하는 얕은 비교를 수행한다.  
* 의존성 배열에 값이 하나라도 변경 사항이 있다면 callback으로 선언한 부수 효과를 실행한다.  
  
**useEffect를 사용할 때 주의할 점**  
  
1. **eslint-disabled-line react-hooks/exhaustive-deps 주석은 최대한 자제하라**  
1. **useEffect의 첫 번째 인수에 함수명을 부여하라**  
1. 거대한 useEffect를 만들지 마라  
1. 불필요한 외부 함수를 만들지 마라  
  
