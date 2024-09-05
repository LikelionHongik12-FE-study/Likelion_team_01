# 쿠키  vs 세션 vs 웹 스토리지

## HTTP 프로토콜의 특징

- **비연결지향**
    
    클라이언트가 요청을 서버에 보내면, 서버는 클라이언트에게 맞는 응답을 보내고 TCP/IP 연결을 끊음. 
    
- **무상태**
    
    연결을 끊는 순간 클라이언트와 서버의 통신이 끝나며 상태 정보를 유지하지 않음. 
    

⇒ 클라이언트가 서버와 연결이 끊겨도 데이터를 기억하기 위해 쿠키, 세션, 웹스토리지 등장.

## Cookie
![image](https://github.com/user-attachments/assets/c463219e-2253-47c8-a0c8-3e9d3aee3c0b)

- 클라이언트에 저장되는 키와 값 형태의 작은 파일로, 이름, 값, 만료시간, 경로정보가 들어있음.
- 클라이언트의 상태 정보를 로컬에 저장했다가 참조.
- 클라이언트에 300개까지 쿠키 저장 가능, 하나의 도메인당 20개의 값을가질 수 있고 하나의 쿠키는 4KB까지 저장 가능.
- 따로 설정하지 않아도 브라우저가 Request 시에 Request Header를 넣어서 자동으로 서버에 전송함.  불필요한 트래픽 양이 증가될 수 있음.
- XSS(HttpOnly 속성 사용하지 않는 경우), CSRF 공격에 취약.
    - XSS : 공격자가 악성 스크립트를 삽입하여 브라우저에서 실행.
    - CSRF : 공격자의 요청이 사용자의 요청인 것처럼 속이는 공격 방식.

- **활용 예시**
    - 비로그인 장바구니
    - N일 동안 보지 않기
    - 로그인 화면에서 ‘아이디 자동완성’ 기능
    - 웹사이트 방문자의 활동을 추적하기 위한 트래킹 데이터 저장

```tsx
import { Cookies } from "react-cookie";

const cookies = new Cookies();

export const setCookie = (name, value, options) => {
  return cookies.set(name, value, {...options})
}

export const getCookie = (name) => {
  return cookies.get(name)
}

export const removeCookie = (name) => {
  return cookies.remove(name);
}
```

## Session
![image (1)](https://github.com/user-attachments/assets/cf0220de-cd74-4965-8e2c-ffd0e62538ba)


- 사용자 정보를 서버측에서 관리.
- 서버에서 클라이언트를 구분하기 위해 세션 ID를 부여하며, 웹 브라우저가 서버에 접속해서 브라우저를 종료할때까지 인증 상태를 유지.
- 데이터를 서버에 두기 때문에 쿠키보다 보안에 좋지만, 사용자가 많아질수록 서버 메모리를 많이 차지함.

## Web Storage

- 클라이언트에 데이터를 저장할 수 있도록 HTML5부터 추가된 저장소.
- Key - Value 스토리지 형태.
- 쿠키와 달리 서버에 자동 전송 위험성이 없음. 쿠키 트래픽 문제 해결.
- 모바일 2.5MB, 데스크탑 5~10MB로 쿠키보다 큰 저장 용량을 가지고 있음.
- 자바스크립트로만 접근 가능하므로 서버가 HTTP 헤더를 통해 스토리지 객체를 조작할 수 없음.
- XSS 공격에 취약.

```tsx
// 로컬스토리지 사용 예시
localStorage.setItem('username', 'JohnDoe');
console.log(localStorage.getItem('username'));
localStorage.removeItem('username');

// 세션스토리지 사용 예시
sessionStorage.setItem('username', 'JohnDoe');
console.log(sessionStorage.getItem('username'));
sessionStorage.removeItem('username');

```

### LocalStorage

- 사용자가 데이터를 지우지 않는 이상 브라우저나 OS를 종료해도 계속 브라우저에 남아있음.
- 오리진(=프로토콜 + 도메인 + 포트)별로 생성됨.

- **활용 예시**
    - 글 임시 저장
    - 사용자 설정 저장 (다크모드)
    - 로그인 화면에서 ‘자동 로그인' 기능

### SessionStroage

- 윈도우나 브라우저 탭을 닫을 경우 제거됨. 현재 떠 있는 탭 내에서만 유지됨.
- 탭/윈도우 단위로 생성되고, 동일한 탭/윈도우라도 다른 오리진(=프로토콜 + 도메인 + 포트)이라면 또 다른 세션 스토리지가 생성됨.

- **활용 예시**
    - 임시로 입력 폼 저장
    - 일회성 로그인
    - 이전 페이지 저장
