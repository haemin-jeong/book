# 타임리프 정리

## 텍스트 출력 
### 태그 속성을 사용한 텍스트 출력
- th:text : 이스케이프 처리
- th:utext : 이스케이프 처리X

### HTML 태그 안에서 직접 출력하기
- [[${data}]] : 이스케이프 처리
- [(${data})] : 이스케이프 처리X

HTML 엔티티 : 웹브라우저는 < 와 같은 특수문자를 HTML 태그로 인식하기 떄문에 < 같은 경우 &lt 를 사용하여 문자를 표현하는데 이것을 HTML 엔티티라고 한다.

이스케이프(escape) : HTML에서 사용하는 특수문자를 HTML 엔티티로 변환해주는 것을 이스케이프라고 한다.

```html
text vs utext

th:text = <span th:text="${data}"></span>
th:utext = <span th:utext="${data}"></span>


[[...]] vs [(...)]

<span th:inline="none">[[...]] = </span>[[${data}]]
<span th:inline="none">[(...)] = </span>[(${data})]
```

## 변수 표현식
- 타임리프는 변수를 사용할 때 변수표현식을 사용한다.
- 변수 표현식은 스프링이 제공하는 SpringEL 표현식을 사용한다.
- 변수 표현식 : ${...} 

### 객체
- ${user.username}
- ${user['username']}
- ${user.getUsername()}

### 리스트
- ${list[index].username}
- ${list[index]['username']}
- ${list[index].getUsername()}

### 맵
- ${map['key'].username}
- ${map['key']['username']}
- ${map['key'].getUsername()}

### 지역 변수
- th:with 을 사용하여 지역변수를 선언하여 사용할 수 있다.
- 지역 변수는 해당 블럭 내에서만 유효하다.
```html
<div th:with="first=${user}"> -- first 변수에 user 객체를 할당
    <p>My name is <span th:text=${first.username}></span></p>
</div>
```

## 기본 객체들
타임리프는 편의를 위해 기본적인 객체들을 제공한다.
- ${#request} : request 객체
- ${#reponse} : response 객체
- ${#session} : session 객체(사용 예: session.data)
- #{#servletContext}
- #{#locale}
- ${param} : HTTP 요청 파라미터 접근(사용 예: url: ...?key=a -> ${param.key})
- #{@beanName.methodName()} : 스프링 빈에 접근(사용 예: helloBean.hello(String data) -> #{@helloBean.hello('Spring')})

## 유틸리티 객체와 날짜
타임리프는 아래와 같은 다양한 유틸리티 객체들을 제공한다.

각각의 사용법은 필요시에 공식 문서를 참고하자.

- #message : 메시지, 국제화 처리
- #uris :  URI 이스케이스 지원
- #dates : java.util.Date 지원
- #calendars : java.util.Calendar 지원
- #temporals : 자바8 날짜 지원(LocalDate, LocalDateTime, Instant)
- #numbers : 숫자 서식 지원
- #strings : 문자 관련 편의 기능 제공
- #objects : 객체 관련 기능 제공
- #bools : boolean 관련 기능 제공
- #arrays : 배열 관련 기능 제공
- #lists, #sets, #maps : 컬렉션 관련 기능 제공
- #ids : 아이디 처리 관련 기능 제공
