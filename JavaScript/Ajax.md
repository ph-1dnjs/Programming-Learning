# Ajax<br>  
  
## Ajax란?<br>  
> Ajax란 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식을 말한다.  
  
  
* Ajax는 브라우저에게 제공하는 Web API인 `XMLHttpRequest` 객체를 기반으로 동작한다.  
* `XMLHttpRequest`는 HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공한다.  
  
이전의 웹페이지는 화면이 전환되면 서버로부터 새로운 HTML을 전송받아 웹페이지 전체를 처음부터 다시 렌더링했다.  
  
Ajax가 등장하면서 서버로부터 웹페이지의 변경에 필요한 데이터만 비동기 방식으로 전송받아 변경할 필요가 없는 부분은 다시 렌더링하지 않고, 변경할 필요가 있는 부분만 렌더링하는 방식이 가능해졌다.  
  
### Ajax 장점<br>  
1. 변경할 부분을 갱신하는데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않는다.  
1. 변경할 필요가 없는 부분은 다시 렌더링하지 않는다. 따라서 화면이 순간적으로 깜빡이는 현상이 발생하지 않는다.  
1. 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않는다.  
  
---  
## JSON<br>  
> JSON은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다. 대부분의 프로그래밍 언어에서 사용할 수 있다.  
  
  
### JSON 표기 방식<br>  
JSON은 키와 값으로 구성된 순수한 텍스트다.  
```javascript  
{
	"name": "Lee",
	"age": 20,
	"alive": true,
	"hobby": ["traveling", "tennis"]
}  
```  
* JSON 키는 반드시 큰따옴표로 묶어야 한다.  
* 값은 객체 리터럴과 같은 표기법을 사용할 수 있다. 하지만 문자열은 반드시 큰따옴표로 묶어야 한다.  
  
### JSON.stringify<br>  
`JSON.stringify` 메서드는 객체를 JSON 포맷의 문자열로 변환한다. 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 하는데 이를 **직렬화**라 한다.  
  
* `JSON.stringify` 메서드는 객체뿐만 아니라 배열도 JSON 포맷의 문자열로 변환한다.  
  
### JSON.parse<br>  
`JSON.parse` 메서드는 JSON 포맷의 문자열을 객체로 변환한다. 서버로부터 클라이언트에게 전송된 JSON 데이터는 문자열이다. 이 문자열을 객체로서 사용하려면 JSON 포맷의 문자열을 객체화해야 하는데 이를 **역직렬화**라 한다.  
  
* 배열이 JSON 포맷의 문자열로 변환되어 있는 경우 `JSON.parse`는 문자열을 배열 객체로 변환한다.  
* 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환한다.  
  
---  
## XMLHttpRequest<br>  
> 자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다. HTTP 요청 전송과 HTTP 응답 수신을 위한 다양한 메서드와 프로퍼티를 제공한다.  
  
  
### XMLHttpRequest 객체 생성<br>  
`XMLHttpRequest` 생성자 함수를 호출하여 생성한다.  `XMLHttpRequest` 객체는 브라우저에서 제공하는 Web API 이므로 브라우저 환경에서만 정상적으로 실행된다.  
  
```javascript  
// XMLHttpRequest 객체의 생성
const xhr = new XMLHttpRequest();  
```  
  
---  
### XMLHttpRequest 객체의 프로퍼티와 메서드<br>  
  
**XMLHttpRequest 객체의 프로토타입 프로퍼티**  
|프로토타입 프로퍼티|설명|  
|:---|:---|
|readyState|HTTP 요청으로 현재 상태를 나타내는 정수. 다음과 같은 `XMLHttpRequest`의 정적 프로퍼티를 값으로 갖는다.<br><br>- UNSENT: 0<br>- OPENED: 1<br>- HEADERS_RECEIVED: 2<br>- LOADING: 3<br>- DONE: 4|  
|status|HTTP 요청에 대한 응답 상태(HTTP 상태 코드)를 나타내는 정수<br>ex) 200|  
|statusText|HTTP 요청에 대한 응답 메세지를 나타내는 문자열<br>ex) “OK”|  
|responseType|HTTP 응답 타입<br>ex) document, json, text, blob, arraybuffer|  
|response|HTTP 요청에 대한 응답 몸체. `responseType`에 따라 타입이 다르다.|  
|reponseText|서버가 전송한 HTTP 요청에 대한 응답 문자열|  
  
**XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티**  
|프로토타입 프로퍼티|설명|  
|:---|:---|
|onreadystatechange|`readyState` 프로퍼티 값이 변경된 경우|  
|onloadstart|HTTP 요청에 대한 응답을 받기 시작한 경우|  
|onprogress|HTTP 요청에 대한 응답을 받는 도중 주기적으로 발생|  
|onabort|`abort` 메서드애 의해 HTTP 요청이 중단된 경우|  
|onerror|HTTP 요청에 에러가 발생한 경우|  
|onload|HTTP 요청이 성공적으로 완료한 경우|  
|ontimeout|HTTP 요청 시간이 초과한 경우|  
|onloadend|HTTP 요청이 완료한 경우. HTTP 요청이 성공 또는 실패하면 발생|  
  
**XMLHttpRequest 객체의 메서드**  
|프로토타입 프로퍼티|설명|  
|:---|:---|
|open|HTTP 요청 초기화|  
|send|HTTP 요청 전송|  
|abort|이미 전송된 HTTP 요청 중단|  
|setRequestHeader|특정 HTTP 요청 헤더의 값을 설정|  
|getResponseHeader|특정 HTTP 요청 헤더의 값을 문자열로 반환|  
  
**XMLHttpRequest 객체의 정적 프로퍼티**  
|프로토타입 프로퍼티|값|설명|  
|:---|:---|:---|
|UNSENT|0|`open` 메서드 호출 이전|  
|OPENED|1|`open` 메서드 호출 이후|  
|HEADERS_RECEIVED|2|`send` 메서드 호출 이후|  
|LOADING|3|서버 응답 중(응답 데이터 미완성 상태)|  
|DONE|4|서버 응답 완료|  
  
### HTTP 요청 전송<br>  
HTTP 요청을 전송하는 경우 다음 순서를 따른다.  
1. `XMLHttpRequest.prototype.open` 메서드로 HTTP 요청을 초기화한다.  
1. 필요에 따라 `XMLHttpRequest.prototype.setRequestHeader` 메서드로 특정 HTTP 요청 헤더 값을 설정한다.  
1. `XMLHttpRequest.prototype.send` 메서드로 HTTP 요청을 전송한다.  
  
```javascript  
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', '/users');

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MINE 타입 지정: json
xhr.setRequestHeader('content-type', 'application/json');

// HTTP 요청 전송
xhr.send();  
```  
  
**XMLHttpRequest.prototype.open**  
`open` 메서드는 서버에 전송할 HTTP 요청을 초기화한다.  
  
```javascript  
xhr.open(method, url[, async]  
```  
  
|매개변수|설명|  
|:---|:---|
|method|HTTP 요청 메서드(“GET”, “POST”, “PUT”, “DELETE” 등)|  
|url|HTTP 요청을 전송할 URL|  
|async|비동기 요청 여부. 옵션으로 기본값은 true이며, 비동기 방식으로 동작한다.|  
  
**HTTP 요청 메서드**  
클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법이다. 주로 5가지 요청 메서드를 사용하여 CRUD를 구현한다.  
   
|HTTP 요청 메서드|종류|목적|페이로드|  
|:---|:---|:---|:---|
|GET|index/retrieve|모든/특정 리소스 취득|X|  
|POST|create|리소스 생성|O|  
|PUT|replace|리소스의 전체 교체|O|  
|PATCH|modify|리소스의 일부 수정|O|  
|DELETE|delete|모든/특정 리소스 삭제|X|  
  
  
  
**XMLHttpRequest.prototye.send**  
`send` 메서드는 `open` 메서드로 초기화된 HTTP 요청을 서버에 전송한다.  
  
* GET 요청 메서드의 경우 데이터를 URL의 일부분인 쿼리 문자열로 서버에 전송한다.  
* POST 요청 메서드의 경우 데이터(페이로드)를 요청 몸체에 담아 전송한다.  
  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/ph-1dnjs/images/6b2fde07-deba-4993-9355-d8cfba40a5fb-88f4dc19-7c31-41ad-af3b-3d1dd4dadcf5.png)  
  
`send` 메서드에는 요청 몸체에 담아 전송할 데이터(페이로드)를 인수로 전달할 수 있다. 페이로드가 객체인 경우 반드시 `JSON.stringify` 메서드를 사용하여 직렬화한 다음 전달해야 한다.  
  
```javascript  
xhr.send(JSON.stringify({ id: 1, content: 'HTML', completed: false }))  
```  
  
  
  
**XMLHttpRequest.prototype.setRequestHeader**  
`setRequestHeader` 메서드는 특정 HTTP 요청의 헤더 값을 설정한다. `setRequestHeader` 메서드는 반드시 `open` 메서드를 호출한 이후에 호출해야 한다.  
  
`Content-type`은 요청 몸체에 담아 전송할 데이터의 MIME 타입의 정보를 표현한다.  
|MIME 타입|서브타입|  
|:---|:---|
|text|text/plain, text/html, text/css, text/javascript|  
|application|application/json, application/x-www-form-urlencode|  
|multipart|multipart/formed-data|  
  
```javascript  
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('POST', '/users');

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MINE 타입 지정: json
xhr.setRequestHeader('content-type', 'application/json');

// HTTP 요청 전송
xhr.send(JSON.stringify({ id: 1, content: 'HTML', completed: false });  
```  
  
MINE 타입을 `“accept”`로 지정할 수 있다. 만약 헤더를 설정하지 않으면 `send` 메서드가 호출될 때 Accept 헤더가 `*/*` 으로 전송된다.  
  
### HTTP 응답 처리<br>  
서버가 전송한 응답을 처리하려면 `XMLHttpRequest` 객체가 발생시키는 이벤트를 캐치해야만 한다.  
  
* 이벤트 핸들러 프로퍼티 중에서 Http 요청의 현재 상태를 나타내는 `readyState`   
* 프로퍼티 값이 변경된 경우엔 `readystatechange` 이벤트를 캐치하여 처리할 수 있다  
  
  
  
  
```javascript  
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

// HTTP 요청 전송
xhr.send();

// readystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티가
// 변경될 때마다 발생한다.
xhr.onreadystatechange = () => {
	// readyState 프로퍼티는 HTTP 요청의 현재 상태를 나타낸다.
	// readyState 프로퍼티 값이 4(XMLHttpRequest.DONE)가 아니면 서버 응답이 완료되지 않은 상태다.
	// 만약 서버 응답이 아직 완료되지 않았다면 아무런 처리를 하지 않는다.
	if (xhr.readyState !== XMLHttpRequest.DONE) return;
	
	// status 프로퍼티는 응답 상태 코드를 나타낸다.
	// status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
	// status 프로퍼티 값이 200이 아니면 에러가 발생한 상태다.
	// 정상적으로 응답된 상태라면 response 프로퍼티에 서버의 응답 결과가 담겨 있다.
	if (xhr.status === 200) {
		console.log(JSON.parse(xhr.response));
		// {userId: 1, id: 1, title: "delete aut autem", completed: false}
	} else {
		console.error('Error', xhr.status, xhr.statusText);
	}
};  
```  
`send` 메서드를 통해 HTTP 요청을 서버에 전송하면 서버는 응답을 반환한다. 하지만 언제 응답이 클라이언트에 도달할지는 알 수 없다. 따라서 `readystatechange` 이벤트를 통해 HTTP 요청의 현재 상태를 확인해야한다. 따라서, `readyState` 프로퍼티가 변경될 때마다 발생한다.  
  
서버의 응답이 완료 되면 Http 요청에 대한 응답 상태 (Http 상태 코드)를 나타내는 `xhr.status`가 200일 때 정상처리를 한다. 그 후 응답으로 받아온 것을 `JSON.parse`를 통해 사용가능하게 만든다.  
  
```javascript  
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

// HTTP 요청 전송
xhr.send();

// load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다.
xhr.onload = () => {
	// status 프로퍼티는 응답 상태 코드를 나타낸다.
	// status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
	// status 프로퍼티 값이 200이 아니면 에러가 발생한 상태다.
	// 정상적으로 응답된 상태라면 response 프로퍼티에 서버의 응답 결과가 담겨 있다.
	if (xhr.status === 200) {
		console.log(JSON.parse(xhr.response));
		// {userId: 1, id: 1, title: "delete aut autem", completed: false}
	} else {
		console.error('Error', xhr.status, xhr.statusText);
	}
};  
```  
`readystatechange` 이벤트 대신 `load` 이벤트를 캐치해도 좋다. `load` 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다. 따라서 `load` 이벤트를 캐치하는 경우 `xhr.readyState`가 `XMLHttpRequest.DONE`인지 확인할 필요가 없다.  
