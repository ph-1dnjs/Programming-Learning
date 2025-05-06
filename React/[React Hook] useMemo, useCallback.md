### useMemo<br>  
> 비용이 큰 연산에 대한 결과를 저장(메모이제이션)해 두고, 이 저장된 값을 반환하는 훅이다.  
  
  
```javascript  
import { useMemo } from 'react';

const memoizedValue = useMemo(() => expensiveComputation(a, b), [a, b]);  
```  
* 첫 번째 인수는 어떠한 값을 반환하는 생성 함수  
* 두 번째 인수는 해당 함수가 의존하는 값의 배열을 전달한다.  
  
  
  
단순히 값뿐만 아니라 컴포넌트도 가능하다.  
```javascript  
function ExpensiveComponent({ value }) {
	useEffect(() => {
		console.log('rendering!');
	})
	return <span>{value + 1000}</span>
}

function App() {
	const [value, setValue] = useState(10);
	
	// 컴포넌트의 props를 기준으로 컴포넌트 자체를 메모이제이션했다.
	const MemoizedComponenet = useMemo(
		() => <ExpensiveComponent value={value} />,
		[value],
	);
	
	const handleChange(e) {
		setValue(Number(e.target.value));
	}
	
	const handleClick() {
		triggerRendering((prev) => !prev)
	}
	
	return (
		<>
			<input value={value} onChange={handleChange} />
			<button onClick={handleClick}>렌더링 발생!</button>
			{MemoizedComponent}
		</>
	)
}  
```  
  
하지만 위 방식은 권장되지 않으며 React.memo를 쓰는 것이 일반적이다.  
```javascript  

const ExpensiveComponent = memo(function ExpensiveComponent({ value }) {
	useEffect(() => {
		console.log('rendering!');
	});
	return <span>{value + 1000}</span>;
});

function App() {
	const [value, setValue] = useState(10);
	const [triggerRendering, setTriggerRendering] = useState(false);

	const handleChange = (e) => {
		setValue(Number(e.target.value));
	};

	const handleClick = () => {
		setTriggerRendering((prev) => !prev);
	};

	return (
		<>
			<input value={value} onChange={handleChange} />
			<button onClick={handleClick}>렌더링 발생!</button>
			<ExpensiveComponent value={value} />
		</>
	);
}
  
```  
  
### useCallback <br>  
> 인수로 넘겨받은 콜백 자체를 기억하는 훅이다. 쉽게 말해 특정 함수를 새로 만들지 않거 다시 재사용한다는 의미다. useMemo가 값의 메모이제이션을 위해 사용했다면, 함수의 메모제이션을 위해 사용하는 것이 useCallback이다.  
  
  
* 첫 번째 인수로 함수를  
* 두 번째 인수로 의존성 배열을 집어 넣는다.  
  
```javascript  
const ChildComponent = memo(({ name, value, onChange }) => {
	// 렌더링이 수행되는지 확인하기 위함
	useEffect(() => {
		console.log('rendering!', name)
	});
	
	return (
		<>
			<h1>{name} {value ? '켜짐' : '꺼짐'}</h1>
			<button onClick={onChange}>toggle</button>
		</>
	);
});

function App() {
	const [status1, setStatus1] = useState(false);
	const [status2, setStatus2] = useState(false);
	
// ---------------------------
// useCallback을 사용하지 않은 경우
// state 값이 바뀌면서 App 컴포넌트가 리렌더링되고, 그때마다 매번 onChange로 넘기는 함수가
// 재생성이되어 클릭하지 않은 다른 컴포넌트도 렌더링된다.
// ---------------------------
	
//	const toggle1 = () => {
//		setStatus1(!status1);
//	};
	
//	const toggle2 = () => {
//		setStatus2(!status2);
//	};


// ---------------------------
// useCallback을 사용한 경우
// ---------------------------

	const toggle1 = useCallback(
		() => {
			setStatus1(!status1);
		},
		[status1],
	);
	
	const toggle2 = usecallback(
		() => {
			setStatus2(!status2);
		},
		[status2],
	);

	
	return (
		<>
			<ChildComponent name="1" value={status1} onChange={toggle1} />
			<ChildComponent name="2" value={status2} onChange={toggle2} />
		</>
	)
}  
```  
  
  
  
**useCallback을 useMemo를 사용해 구현**  
“리액트 공식 문서에서도 확인할 수 있다.”  
```javascript  
export function useCallback(callback, args) {
	return useMemo(() => callback, args);
}  
```  
  
  
  
