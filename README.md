# Chapter 3. 컴포넌트를 이용한 애플리케이션 구축

## 속성 유효성 검사

Component를 만들 때, 컴포넌트를 조합해 더 튼 컴포넌트를 만들고, 재사용할 수 있어야 함
필수값을 속성과 어떤 형식의 값을 받는지 지정이 필요함 이때 propTypes를 선언


#### 속성 유효성 검사의 장점

1. 언제든지 컴포넌트를 열고 어떤 속성이 필요한지, 속성의 형식이 무엇인지 알 수 있음
2. 문제가 발생한 경우 콘솔에 메세지와 문제가 발생한 render 메서드를 출력해줌

```
import React, {Component, PropTypes} from 'react';
import {render} from 'react-dom';

class Greeter extends Component{
  render(){
    return( <h1>{this.props.salutation}</h1>)
  }
}
Greeter.propTypes= {
  salutation:PropTypes.string.isRequired
}

render(<Greeter salutation="world"/>, document.getEIementByld('container'));
```

### 속성 기본값
- 속성 값을 별도로 지정하지 않았을때 이용할 기본값을 지정하려면 defaultProps객체를 생성자 속성으로 정의

```
class Greeter extends Component{
  render(){
    return( <h1>{this.props.salutation}</h1>)
  }
}

Greeter.propTypes= {
  salutation:PropTypes.string.isRequired
}

Greeter.dafulatProps = {
	saluation: "hello world"
}

render(<Greeter />, document.getEIementByld('container'));
```

### 기본 제공되는 propTypes 유효성 검사기
![propTypes](/images/1.png)
![propTypes2](/images/2.png)
![propTypes3](/images/2.png)

### 커스텀 propTypes 유효성 검사기
유효성 검사기는 속성의 리스트, 검사할 속성의 이름, 컴포넌트 이름을 받는 함수이다.
유효한 경우 아무것도 반환하지 않으며 잘못된 경우 Error 인스턴스 반환해야 한다.

## 컴포넌트 조합 전략과 모범 사례
### 상태저장 컴포넌트와 순수 컴포넌트
- props는 컴포넌트의 구성정보에 해당한다. 속성은 상위 컴포넌트로부터 받으며 컴포넌트 내에서는 변경할 수 없다.
- state는 컴포넌의 생성자에 정의된 기본값에서 시작해 변경가능하다. 컴포넌트의 상태를 내부적으로 관리. 상태가 변경되면 다시 렌더링된다.

```
상태 저장 컴포넌트는 말 그대로 state를 가지고 있는 컴포넌트로 순수 컴포넌트는 데이터를 그대로 렌더링 하는 역할만 하는 component
당연하게도 state의 변화에 반응해야하는 상태 저장 컴포넌트보단, 단순한 순수 컴포넌트가 재사용성이나 테스트하기에 수월하기 때문에 순수component를 더 많이 만드는 것이 바람직
```

## 어떤 컴포넌트가 상태 저장이어야 할까?
- 해당하는 상태를 기준으로 무언가를 렌더링하는 모든 컴포넌트를 찾는다.
 - state가 필요하고, 어떠한 이벤트에 변경점이 있는 컴포넌트
- 공통 소유자 컴포넌트를 찾는다. (계층에서 상태를 필요로 하는 모든 컴포넌트의 상위에 있는 단일 컴포넌트)
- 공통 소유자나 계층에서 더 상위에 있는 다른 컴포넌트가 상태를 소유해야 한다.
 - 상위 컴포넌트에서 소유하고 있는 state를 하위에 props로 넘기고 (콜백과 함께), 하위 컴포넌트는 props를 토대로 렌더링하며, 값이 변경되어야 할 경우에는 콜백을 호출하여 다시 상위 컴포넌트의 state를 변경 하는 구조
- 해당 상태를 소유하기에 적절한 컴포넌트를 찾을 수 없는 경우 단순히 상태를 저장하기 위한 컴포넌트를 새로 만들고 계층에서 공통 소유자 컴포넌트 위쪽에 추가한다.
```
1 - 컴포넌트를 내부 상태를 조작하는 상태 저장 컴포넌트와
2 - 내부 상태가 없고 속성을 통해 받은 데이터를 표시하는 일만 하는 순수 컴포넌트의
두 분류로 구분하는 것이 컴포넌트 재시용에 바람직하다.
```

 ## 컴포넌트 수명주기
 ### 수명주기 단계와 메서드
 ![lifeCycle](/images/4.png)
 ![lifeCycle](/images/5.png)
 ![lifeCycle](/images/6.png)

 ## 불변성에 대한 개요
- Component의 내부 상태를 변경하기 위한 setState 메서드를 이용하고 this.state는 변경 불가로 받아들이는 것 이 좋다.
 - 리액트의 상태관리 우회하기 때문에, 패러다임을 위반하게 됨
 - setState()를 호출하면 이전에 수행한 변경이 무효화될 위험성이 있음
 - 애플리케이션 성능 개선 기회가 줄어듬
