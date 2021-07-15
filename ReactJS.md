#   React JS 시작하기
[toc]
## Setup


### Requirements

1. node JS 설치
2. npm 설치
3. npx 설치  npm install npx -g
4. git 설치

### Create React App

다음과 같은 명령어를 통하여 React를 사용하기 위한 필요 파일들을 통합하여 설치할 수 있다.

```
npx create-react-app '폴더명'
```

### Delete unnecessary files after creating react app

npx create-react-app 을 통하여 파일이 생성이 된 후 필요 없는 내용을 지워주는 과정이다.  또한 src 안에 폴더에는 App.js와 index.js 파일을 제외하고 모두 삭제해준다.

```react
//index.js 파일 
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(
  <App />, document.getElementById('root')
);

```

```react
//App.js 파일
import React from 'react';

function App() {
  return (
    <div className="App"></div>
  );
}

export default App;

```

이로써 react 프로젝트를 위한 기본 셋업이 끝났다.



****



## JSX

react는 컴포넌트를 가져와서 브라우저가 이해할 수 있는 평범한 일반 HTML로 만든다. 위 과정에서 컴포넌트(자바스크립트로 쓰여진)안에 html를 삽입하는 과정에서 JSX 문법을 사용하게 되면 굉장히 쉬워진다. 간단하게 말하자면 JSX = JAVASCRIPT + HTML 이다.

```react
// JSX 문법을 적용하지 않은 코드
const element = React.createElement(
	'h1',
	{className: 'title'},
	'Hello, world!'
)

ReactDOM.render(
	element,
	document.getElementByID('root');
)
```

```react
// JSX 문법을 적용한 코드
const element = <h1 className='title'>Hello, world!</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

다음은 [react 공식 홈페이지](https://reactjs.org/)에서 말하고 있는 JSX의 특징이다. 

- JSX의 중괄호 안에는 유효한 모든 JavaScript 표현식을 넣을 수 있다.  
- JavaScript의 자동 세미콜론 삽입을 피하고자 괄호로 묶는 것을 권장한다.
- html 속성에 따옴표를 이용해 문자열 리터럴을 정의할 수 있다.
- 중괄호를 사용하여 html 어트리뷰트에 JavaScript 표현식을 삽입할 수 있다. 
- JSX는 HTML보다는 JavaScript에 가깝기 때문에, React DOM은 HTML 어트리뷰트 이름 대신 `camelCase` 프로퍼티 명명 규칙을 사용한다.
- 태그가 비어있다면 XML처럼 />를 이용해 바로 닫아주어야 한다.
- 주입 공격을 방지한다. (XSS, cross-site-scripting)
- 바벨은 JSX를 만나면 위의 코드처럼 createElement() 호출로 컴파일을 한다



+++



## Components and Props

개념적으로 컴포넌트는 JavaScript 함수와 유사하다. "props"라고 하는 임의의 입력을 받은 후, 화면에 어떻게 표시되는지를 기술하는 React 엘리먼트를 반환한다. 

```react
// 함수 컴포넌트
function Welcome(props){
	return <h1>Hello, {props.name}</h1>;
}
```

```react
// 클래서 컴포넌트
class Welcome extends React.Component{
	render(){
		return <h1>Hello, {this.props.name}</h1>
	}
}
```

컴포넌트의 이름은 항상 **대문자**로 시작한다.

사용자 정의 컴포넌트로 작성한 엘리먼트를 발견하면 JSX 어트리뷰트(따옴표로 이용한 문자열 리터럴)와 자식을 해당 컴포넌트에 단일 객체로 전달한다. 이 객체를 "props"라고 한다. 



다음 예시를 보면 컴포넌트와 컴포넌트 사이의 데이터를 주고 받는 방법 중 하나인 props 와 이를 어떻게 활용하는지를 알 수 있다. 

```react
// 자바스크립트 비구조화를 통하여 props의 name으로 직접 가져올 수 있다. 
//function kimchi({ name }){
//    return <h1> I love {name} </h1>
//}

function Food(props){
	return <h1>I love {props.name} </h1>
}

function App(){
    return(
        <div className="App">
        	<Food name="kimchi"/>
    	</div>
    );
}
```

___

## Map Recap

다음 코드를 보고 문제점에 대해서 얘기해보자. 

```react
// without Map
function Food(props){
	return <h1>I love {props.name} </h1>
}
function App(){
    return(
        <div className="App">
        	<Food name="kimchi"/>
            <Food name="kimbab"/>
            <Food name="bulgogi"/>
            <Food name="ramen"/>
    	</div>
    );
}
```

우리는 컴포넌트와 props를 사용하여 코드를 재활용했지만 만약 이런 중복된 컴포넌트가 단순히 4개가 아니거나, 혹은 다른 api에서 값을 받아서 props에 넘겨줘야 할 상황이 있을 것이다. 그런 경우에는 이런 식의 하드코딩은 좋지 않거나 혹은 표현할 수 없다. 그럴 때 Map 이라는 함수를 통해서 표현할 수 있다. 

우선 map 함수를 보면 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환한다고 한다. mozila에서는 다음과 같이 예시를 들며 설명하고 있다. 

```javascript
// arr.map(callback(currentValue[, index[, array]])[, thisArg])
const array1 = [1, 4, 9, 16];

// pass a function to map
const map1 = array1.map(x => x * 2);

console.log(map1);
// expected output: Array [2, 8, 18, 32]
```

이런식으로 위의 4가지의 음식을 표현해보자

```react
// with Map
// with arrowFunc
const foods = ['kimchi','kimbab','bulgogi','ramen'];
function Food(props){
	return <h1>I love {props.name} </h1>
}

function App(){
    return(
        <div className="App">
        	{foods.map(dish =>(
            	<Food name = dish/>
            ))}
    	</div>
    );
}
```

이렇게 되면 다 잘 될 줄 알았지만 console 창에 들어가보면 에러가 뜰 것이다.

**Each child in a list should have a unique "key" prop.** 이를 해결하기 위해 foods 라는 배열안에 id를 추가할 것이고 이를 key라는 이름의 props로 넘겨주면 각각의 elment가 유일성을 가지면서 에러가 사라질 것이다.

```react
// with Map
// with arrowFunc
const foods = [{
    			name : 'kimchi',
               	id : 1
               },
               {
    			name : 'kimbab',
               	id : 2
               },
              {
    			name : 'bulgogi',
               	id : 3
               },
              {
    			name : 'ramen',
               	id : 4
               }];
function Food(props){
	return <h1>I love {props.name} </h1>
}

function App(){
    return(
        <div className="App">
        	{foods.map(dish =>(
            	<Food name={dish.name} key={dish.id}/>
            ))}
    	</div>
    );
}
```



___

## State

지금까지는 정적인 데이터를 component에 전달하는 방법인 props에 대해서 배웠다. state를 사용하면 이런 문제를 해결할 수 있다. 

```react
class App extends React.Component {
  state = {
    isLoading : true
  };
  componentDidMount(){
    setTimeout(() => {
      this.setState({isLoading: false});
    },3000);
  }
  render(){
    const { isLoading } = this.state;
    return(
      <div>{isLoading? "Loading..." : "We are ready"}</div>
    );
  }
}
```

또한 state를 수정할때는 setState()를 통하여 수정해야만 하고, 직접 수정은 불가하다. 

```react
// this.state.data = 3 ...XX 
class App extends React.Component{
    setState(data : 3);
    render(){
        return ({this.state.data});
    }
}
```

## Component Life Cycle

컴포넌트 클래스에서 특별한 메서드를 선언하여 컴포넌트가 마운트되거나 언마운트 될 때 일부 코드를 작동할 수 있다. 이러한 메서드들은 "생명주기 메서드"라고 불린다. 

```react
// mount : 처음 컴포넌트의 인스턴스가 생성될때 
// update : props 또는 state가 변경되어 갱신될 때

class App extends React.Component {
  state = {
    isLoading : true
  };
  componentDidMount(){
    setTimeout(() => {
      this.setState({isLoading: false});
    },3000);
  }
  render(){
    const { isLoading } = this.state;
    return(
      <div>{isLoading? "Loading..." : "We are ready"}</div>
    );
  }
}
```















