---

title: [React Hook] 1. useState을 알아보자 
date: 2022-8-1
categories: 
    - Frontend
    - React
    - Web

tags:
    - TIL
 
---

React에서 가장 많이 쓰이는 Hook은 아마도 useState일 것이다. 컴포넌트에 상태를 부여할 때 쓰는 hook이다. useState()의 인자로 inital value를 넣어준다. 아무것도 넣지 않으면 상태가 undefined로 초기화 된다. useState 함수는 두 가지를 return 한다. 하나는 상태 값이고 다른 하나는 상태값을 변경시키는 함수이다. 상태 값을 변동시키는 함수에는 관습적으로 setSomething이라는 식별자를 부여한다. 상태값을 변동시킬 때는 항상 이 set 함수를 이용해야 하는데, 상태값 변화에 따라 React가 이를 인지하고 UI를 다시 렌더링하기 때문이다. 또한 그래야지 컴포넌트 함수가 다시 실행되어도 상태가 유지된다. 기본적인 사용법은 다음과 같다.

```javascript
function App() {
    const [value, setValue] = useState("");

    return(
        <div>
            <p>{value}</p>
            <input type="text" value={value} onChange={(e)=>setValue(e.target.value)}/>
        </div>
    );
}
```

일반적으로 set 함수가 실행되면 상태값이 바뀌고 컴포넌트 함수가 다시 실행된다. 이에 따라 React는 가상DOM을 업데이트하고 변화된 부분에 대해서 실제 DOM을 변화시킨다. 하지만 set 함수를 실행한다고 항상 컴포넌트 함수가 다시 실행되는 것은 아니다. set으로 설정된 새로운 상태가 기존 상태와 다를 떄만 컴포넌트 함수를 실행한다.(다른 말로 가상 DOM을 업데이트한다.) 다음과 같은 경우 예상한대로 작동하지 않는데 왜냐하면 set 함수를 사용하기는 했어도 react가 상태값을 비교할 때 같은 object라고 보기 때문에 가상 DOM을 다시 만들지 않고 따라서 다시 렌더링을 하지 않기 때문이다.

```javascript
function App() {
  const [state, setState] = useState({ inputVal: "" });

  return(
      <div>
          <p>{state.inputVal}</p>
          <input 
            type="text" 
            value={state.inputVal} 
            onChange={(e)=>{
              state.inputVal = e.target.value;
              setState(state)
            }}
          />
      </div>
  );
}
```

따라서 상태 값이 object일 때는 새로운 object 만들어서 넣어주는 형식으로 상태 값을 변화시켜야한다. 이것을 object를 immutable한 것 처럼 여긴다고 말하기도 한다. 다음처럼 전개 구문을 활용하면 보다 쉽게 새로운 객체를 만들 수 있다. 추가적으로 상태 값이 객체인 경우 useReducer를 활용하면 보다 쉽게 상태값을 관리할 수 있는데 이는 추후에 다루려고 한다.

```javascript
function App() {
  const [state, setState] = useState({ inputVal: "" });

  return(
      <div>
          <p>{state.inputVal}</p>
          <input 
            type="text" 
            value={state.inputVal} 
            onChange={(e)=>{
              setState({...state, inputVal: e.target.value})
            }}
          />
      </div>
  );
}
```

useState와 같은 훅을 사용하다보면 useState 함수가 컴포넌트 함수가 실행될 때마다 실행되는 것을 알 수 있다. 그러면서도 상태값은 유지를 시켜준다. useState 함수가 매번 실행되면서도 상태값을 잘 유지한다는 것이 신기해서 이것이 어떻게 구현되는 것인지 찾아봤다. 한 유투브 영상을 통해 다음과 같은 구조로 상태를 관리한다는 것을 알 수 있었다. useState는 클로져로 상위 lexical 환경에서 관리되는 state 값을 index로 참조하여 매번 적절한 상태값을 return해주는 것이다. 이러한 방식 때문에 useState는 사용법에 주의해야한다. if문이나 for문 등에서 useState를 호출하면 조건에 따라 useState가 호출되는 순서나 횟수가 바뀌게 되고 상태가 뒤죽박죽이 되기 떄문이다. 이러한 오류를 방지하고자 react는 useState는 함수가 컴포넌트의 최상위 환경이나 커스텀 훅 최상위 환경에서만 호출될 수 있도록 제한하고 있다.

(여기서 드는 의문점: conditional rendering의 경우도 state의 접근 index에 영향을 줄 수 있을거 같은데 왜 문제가 되지 않을까? 흠.. component별로 state 관리가 나뉘어져 있나?)

```javascript
const ReactX = (() => {
    let state = [];
    let index = 0;

    const useState = (initalValue) => {
        const localIndex = index; 
        index++;

        if(state[localIndex] === undefined){
            state[localIndex] = initalValue;
        }

        const setterFunction = (newValue) => {
            state[localIndex] = newValue;
        }

        return [state[localIndex],  setterFunction];
    }

    return {
        useState
    };
})();

```


[😶 hook의 작동방식을 설명하는 유투브 영상](https://www.youtube.com/watch?v=1VVfMVQabx0)