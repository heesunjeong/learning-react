# Chapter 3. 컴포넌트를 이용한 애플리케이션 구축

## 속성 유효성 검사
```
Component를 만드는 이유: 컴포넌트를 조합해 더 튼 컴포넌트를 만들고, 재사용할 수 있어야 함
=> 필수값을 속성과 어떤 형식의 값을 받는지 지정이 필요함
이때 propTypes를 선언
```
1. 언제든지 컴포넌트를 열고 어떤 속성이 필요한지, 속성의 형식이 무엇인지 알 수 있다
2. 문제가 발생한 경우 콘솔에 메세지와 문제가 발생한 render 메서드를 출력해줌

### 속성 기본값
- 속성 값을 별도로 지정하지 않았을때 이용할 기본값을 지정하려면 defaultProps객체를 생성자 속성으로 정의

### 기본 제공되는 propTypes 유효성 검사기
사진넣기

### 커스텀 propTypes 유효성 검사기
유효성 검사기는 속성의 리스트, 검사할 속성의 이름, 컴포넌트 이름ㅇ르 받는 함수이다.
유효한 경우 아무것도 반환하지 않으며 잘못된 경우 Error 인스턴스 반환해야 한다.

## 컴포넌트 조합 전략과 모범 사례
### 상태저장 컴포넌트와 순수 컴포넌트
- props는 컴포넌트의 구성정보에 해당한다. 속성은 상위 컴포넌트로부터 받으며 컴포넌트 내에서는 변경할 수 없다.
- state는 컴포넌의 생성자에 정의된 기본값에서 시작해 변경가능하다. 컴포넌트의 상태를 내부적으로 관리. 상태가 변경되면 다시 렌더링된다.

```
상태 저장 컴포넌트는 말 그대로 state를 가지고 있는 컴포넌트 이구요. 순수 컴포넌트는 데이터를 그대로 렌더링 하는 역할만 하는 컴포넌트입니다.
당연하게도 state의 변화에 반응해야하는 상태 저장 컴포넌트보단, 단순한 순수 컴포넌트가 재사용성이나 테스트하기에 수월 하겠죠!!. 그래서 가능하다면 컴포넌트 대부분은 순수 컴포넌트가 좋다구해요.
```

## 어떤 컴포넌트가 상태 저장이어야 할까?
- 해당하는 상태를 기준으로 무언가를 렌더링하는 모든 컴포넌트를 찾는다.
 - state가 필요하고, 어떠한 이벤트에 변경점이 있는 컴포넌트를 찾으라는거 같죠?

- 공통 소유자 컴포넌트를 찾는다. (계층에서 상태를 필요로 하는 모든 컴포넌트의 상위에 있는 단일 컴포넌트).
 - 1번에서 찾은 컴포넌트들의 상위에 있는 단일 컴포넌트.

- 공통 소유자나 계층에서 더 상위에 있는 다른 컴포넌트가 상태를 소유해야 한다.
 - 요건 책을 볼게요. 79페이지. 상위 컴포넌트에서 소유하고 있는 state를 하위에 props로 넘기고 (콜백과 함께), 하위 컴포넌트는 props를 토대로 렌더링 하며, 값이 변경되어야 할 경우에는 콜백을 호출하여 다시 상위 컴포넌트의 state를 변경 하는 구조의 내용입니다.

- 해당 상태를 소유하기에 적절한 컴포넌트를 찾을 수 없는 경우 단순히 상태를 저장하기 위한 컴포넌트를 새로 만들고 계층에서 공통 소유자 컴포넌트 위쪽에 추가한다.
 - 79페이지 처럼 딱 떨어지는 경우? 가아니면 래퍼를 하나 만들어서 적용시키라는 말 같은데요. 경험이 있어야 확실한 상황을 알 것 같네요

 ## 컴포넌트 수명주기
 ### 수명주기 단계와 메서드
