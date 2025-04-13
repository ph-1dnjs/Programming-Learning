# Set과 Map<br>  
  
## Set<br>  
> Set 객체는 중복되지 않는 유일한 값들의 집합이다.  
  
  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/ph-1dnjs/images/d5ff0b76-e7e1-425d-8e3d-afa6a2f84d30-image.png)  
  
  
  
### Set 객체의 생성<br>  
Set 객체는 Set 생성자 함수로 생성한다.   
```javascript  
// 생성자 함수에 인수를 전달하지 않으면 빈 Set 객체가 생성된다.
const set = new Set();
console.log(set); // Set(0) {}

// Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다.
// 중복된 값은 Set 객체에 요소로 저장되지 않는다
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

// 중복을 허용하지 않는 Set 객체의 특징 활용
// 배열에서 중복된 요소 제거하기
const uniq = array => array.filter((v, i, self) => self.indexOf(v) === i);
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]

const uniq2 = array => [...new Set(array)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]  
```  
  
### 요소 개수 확인 <br>  
Set 객체의 요소 개수를 확인할 때는 Set.prototype.size 프로퍼티를 사용한다.  
```javascript  
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // ?  
```  
* size 프로퍼티는 getter 함수만 존재하는 접근자 프로퍼티다 . 따라서 size 프로퍼티로 값을 변경할 수 없다.  
  
### 요소 추가<br>  
Set 객체에 요소를 추가할 때는 Set.prototype.add 메서드를 사용한다.  
```javascript  
const set = new Set();
console.log(set); // Set(0) {}

set.add(1);
console.log(set); // Set(1) {1}  
```  
* add 메서드는 새로운 요소가 추가된 Set 객체를 반환한다. ⇒ 연속 호출이 가능하다.  
* 중복된 요소인 경우 에러가 발생하지 않고 무시한다.  
* Set은 NaN 요소를 같다고 평가하여 중복 추가를 허용하지 않는다. (음수 0과 양수 0도 마찬가지)  
  
```javascript  
const set = new Set();

set
	.add(1)
	.add('a')
	.add(true)
	.add(undefined)
	.add(null)
	.add({})
	.add([])
	.add(() => {});
	
	console.log(set); // Set(8) {1, 'a', true, undefined, null, {}, [], () => {}}
}  
```  
* Set 객체는 객체나 배열과 같이 자바스크립트의 모든 값을 요소로 저장할 수 있다.  
  
### 요소 존재 여부 확인<br>  
Set 객체에 특정 요소가 존재하는지 확인하려면 Set.prototype.has 메서드를 사용한다.  
  
```javascript  
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false  
```  
  
### 요소 삭제<br>  
Set 객체의 특정 요소를 삭제하려면 Set.prototype.delete 메서드를 사용한다.  
  
```javascript  
const set = new Set([1, 2, 3]);

set.delete(2);
console.log(set); // Set(2) {1, 3}

set.delete(0);
console.log(set); // Set(2) {1, 3}  
```  
* delete 메서드에는 인덱스가 아니라 요소값을 인수로 전달해야 한다.  
* Set 객체는 순서에 의미가 없다. ⇒ 배열과 같이 인덱스를 갖지 않는다.  
* 존재하지 않는 요소를 삭제하려하면 에러 없이 무시된다.  
* delete 메서드는 불리언 값을 반환한다. ⇒ 연속적으로 호출할 수 없다.  
  
### 요소의 일괄 삭제<br>  
Set 객체의 모든 요소를 일관 삭제하려면 Set.prototype.clear 메서드를 사용한다.  
  
```javascript  
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set(0) {}  
```  
* clear 메서드는 언제나 undefined를 반환한다.  
  
### 요소 순회<br>  
Set 객체의 요소를 순회하려면 Set.prototype.forEach 메서드를 사용한다.  
  
Set.prototype.forEach 메서드의 콜백 함수는 다음과 같이 3개의 인수를 전달받는다.  
* 첫 번째 인수: 현재 순회 중인 요소값  
* 두 번째 인수: 현재 순회 중인 요소값  
* 세 번째 인수: 현재 순회 중인 Set 객체 자체  
  
  
  
```javascript  
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
/*
1 1 Set(3) {1, 2, 3}
2 2 Set(3) {1, 2, 3}
3 3 Set(3) {1, 2, 3}
*/  
```  
* Set 객체는 이터러블이다. ⇒ 따라서 for … of 문으로 순회할 수 있고, 스프레드 문법, 배열 디스트럭처링의 대상이 될 수도 있다.  
  
## Map<br>  
---  
> Map 객체는 키와 값의 쌍으로 이루어진 컬렉션이다.  
  
  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/ph-1dnjs/images/ded7555f-5bd6-4b70-8d50-8027def2e99c-image.png)  
  
### Map 객체의 생성<br>  
Map 객체는 Map 생성자 함수로 생성한다.   
```javascript  
// Map 생성자 함수에 인수를 전달하지 않으면 빈 Map 객체가 생성된다.
const map = new Map();
console.log(map); // Map(0) {}

// Map 생성자 함수는 이터러블을 인수로 받아 Map 객체를 생성한다.
// 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.
const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

// 중복된 키를 갖는 요소가 존재하면 값이 덮어써진다.
// 따라서 중복된 키를 갖는 요소가 존재하지 않는다.
const map2 = new Map([['key1', 'value1'], ['key1', 'value2']]);
console.log(map2); // Map(1) {"key1" => "value1"}  
```  
  
### 요소의 개수 확인<br>  
Map 객체의 요소 개수를 확인할 때는 Map.prototype.size 프로퍼티를 사용한다.  
  
### 요소 추가<br>  
Map 객체의 요소를 추가 할때는 Map.prototype.set 메서드를 사용한다.  
  
  
  
### 요소 취득<br>  
Map 객체에서 특정 요소를 취득하려면 Map.prototype.get 메서드를 사용한다.   
  
* get 메서드의 인수로 키를 전달하면 해당 키를 갖는 값을 반환한다.  
* 요소가 존재하지 않으면 undefined를 반환한다.  
  
### 요소 존재 여부 확인<br>  
Map 객체에 특정 요소가 존재하는지 확인하려면 Map.prototype.has 메서드를 사용한다.  
  
### 요소 삭제<br>  
Map 객체의 요소를 삭제하려면 Map.prototype.delete 메서드를 사용한다.  
  
### 요소 일괄 삭제<br>  
Map 객체의 요소를 일괄 삭제하려면 Map.prototype.clear 메서드를 사용한다.  
  
### 요소 순회<br>  
Map 객체의 요소를 순회하려면 Map.prototype.forEach 메서드를 사용한다.  
  
📌 Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다.  
![IMAGE](https://raw.githubusercontent.com/nogi-bot/resources/main/ph-1dnjs/images/f453ea62-0274-4c80-8557-1f6033c1595a-image.png)  
  
  
  
