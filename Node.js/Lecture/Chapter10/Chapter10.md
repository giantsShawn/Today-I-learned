# 만든 백앤드 API를 가지고 프론트앤드와 연결하는 실습
1. React를 MongoDb , Express Node.js에 연결
2. 수신한 응답 데이터를 리액트에서 사용하는방법
3. 리액트상에서 오류를 핸드링 하는방법

## 프론트엔드 유효성검사는 사용자에 의해 활성화될수도있어 백앤드에서 예외처리를 해줘야한다

> 핵심 프론트 앤드 코드 Auth.js

## useEffect 사용시 async 를 붙이지 마라!
- 프로미스를반환하는 함수나 비동기함수가 필요가 없다

## AbortController 
- 비동기 작업을 중단하기 위해 사용된다
- fetch api에 signal을 넣어서 사용

- 컴포넌트 단에서 사용 유무
  - 서버로부터 데이터를 가져오는도중에 엄청빨리 다른페이지로 넘어갈경우에 더이상 화면에 없는 컴포넌트에서 상태를 업데이트 할수있으므로 진행중인 HTTP 요청을 취소하기위해 사용된다.