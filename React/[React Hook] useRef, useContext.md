### useRef<br>  
> useRef는 컴포넌트가 리렌더링돼도 유지되는 변경 가능한 값을 저장하거나 DOM 요소에 직접 접근할 때 사용하는 훅이다.   
  
  
useState와 동일하지만 큰 차이점 두 가지를 가지고 있다.  
  
1. useRef는 반환값인 객체 내부에 있는 current로 값에 접근 또는 변경할 수 있다.  
1. useRef는 그 값이 변하더라도 렌더링을 발생시키지 않는다.  
  
  
  
### useContext<br>  
> Context와 해당 Context를 함수 컴포넌트에서 사용할 수 있게 해주는 훅이다.  
  
  
  
  
```javascript  
const Context = createContext<{ hello: string } | undefined>(undefined)

function ParentComponent() {
	return (
		<>
			<Context.Provider value={{ hello: 'react' }}>
				<Context.Provider value={{ hello: 'javascript' }}>
					<ChildComponent />
				</Context.Provider>
			</Context.Provider>
		</>
	)
}

function ChildComponent() {
	const value = useContext(Context)
	
	// react가 아닌 javascript가 반환된다.
	return <>{value ? value.hello : ''}</>
}  
```  
* 여러 개의 Provider가 있다면 가장 가까운 Provider의 값을 가져오게 된다.  
  
**useContext를 사용할 때 주의할 점**  
  
  
