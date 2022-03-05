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

## URL 링크
타임리프에서 URL을 사용할 때는 @{...} 을 사용한다.

절대 경로, 상대 경로 모두 사용할 수 있다.

- 단순 경로 : @{/exam/hello/1}
- 경로 변수 사용 : @{/exam/{var1}/{var2}(var1='data1', var2='data2')} -> /exam/data1/data2
- 쿼리 파라미터 사용 : @{/exam(param1='data1', param2='data2')} -> /exam?param1=data1&param2=data2
- 경로 변수 + 쿼리 파라미터 : @{/exam/{param1}(param1='data1', param2='data2')} -> /exam/data1?param2=data2

## 문자 리터럴
타임리프에서 문자 리터럴은 항상 ' 으로 감싸줘야하며 공백 없이 이어지는 문자열의 경우엔 생략이 가능하다.

예시
- th:text="hello world" -> X  
- th:text="'hello'" -> O
- th:text="'hello world'" -> O
- th:text="hello" -> O

문자열에 변수 출력하기
- 일반적인 방법 : th:text="'hello ' + ${data}"
- 리터럴 대체 : th:text="|hello ${data}|"

## 속성 값 설정
### 속성 설정
`th:속성명` 로 속성 값을 지정하면, 기존에 해당 속성명이 HTML에 있는 경우 덮어씌워지고, 없는 경우 생성 된다.

사용 예
- 렌더링 전: `<input name="hello" th:name="hi"/>` -> 렌더링 후: `<input name="hi"/>`
- 렌더링 전: `<input th:name="hi"/>` -> 렌더링 후: `<input name="hi"/>`

### 기존 속성에 값 추가
- th:attrappend="속성명=' 속성값'" : 해당 속성의 속성 값 뒤에 이어 붙힌다.(공백 주의, 공백이 없으면 기존 클래스명에 공백 없이 이어 붙혀진다)
  - 렌더링 전: `<span class="c1" th:attrappend="class=' c2'"/>` -> 렌더링 후: `<span class="c1 c2">`
- th:attrprepend="속성명='속성값 '" : 해당 속성의 값 앞에 이어 붙힌다.(공백 주의, 공백이 없으면 기존 클래스명에 공백 없이 이어 붙혀진다)
  - - 렌더링 전: `<span class="c1" th:attrprepend="class='c2 '"/>` -> 렌더링 후: `<span class="c2 c1">`
- th:classappend="클래스명" : class 속성에 값을 추가한다.

### check 속성 처리
HTML의 checkbox에서 check 여부는 단순히 checked 옵션 유무로 판단한다.
- check : `<input type="checkbox" checked/>`
- uncheck : `<input type="checkbox"/>`

타임리프에서 checkbox check여부를 boolean 값을 사용해서 판단한다.
- check : `<input type="checkbox" th:checked="true">` -> `<input type="checkbox" checked/>`
- uncheck : `<input type="checkbox" th:checked="false">` -> `<input type="checkbox"/>`

## 반복
타임리프에서 `th:each`를 사용해서 List와 같은 컬렉션에 있는 값을 하나씩 꺼내서 사용할 수 있다.

List, Set, 배열 등 Iterable, Enumeration을 구현한 모든 객체를 반복에 사용할 수 있다.

Map의 경우에는 [Map.Entry](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Map.Entry.html)가 반복 객체에 사용된다.

문법 : `th:each="변수명 : ${컬렉션명}"`

### 예제 코드
Controller
```java
@Controller
public class MenuController {

    @GetMapping("/menu")
    public String menu(Model model) {
        List<Menu> list = new ArrayList<>();

        list.add(new Menu("불고기", 8000));
        list.add(new Menu("삽겹살", 12000));
        list.add(new Menu("목살", 10000));

        model.addAttribute("menus", list);

        return "basic/menu";
    }

    static class Menu {
        String name;
        int price;

        public Menu(String name, int price) {
            this.name = name;
            this.price = price;
        }

        public String getName() {
            return name;
        }

        public int getPrice() {
            return price;
        }
    }
}

```
HTML
```html
<table>
  <tr>
    <th>메뉴</th>
    <th>가격</th>
  </tr>
  <tr th:each="menu : ${menus}">
    <td th:text="${menu.name}"></td>
    <td th:text="${menu.price}"></td>
  </tr>
</table>
```
실행 결과
|메뉴|가격|
|---|---|
|불고기|8000|
|삼겹살|12000|
|목살|10000|

### 편의 기능 - 상태 정보
`th:each`에서 두번째 파라미터를 설정해서 index, size 등과 같은 반복의 상태 정보를 사용할 수 있다.

두번째 파라미터는 생략이 가능하며, 생략 가능 조건은 사용 시에 '변수명 + Stat' 이름으로 상태 정보에 접근하는 경우이다.

상태 정보
- index : 0부터 시작하는 인덱스
- count : 1부터 시작하는 값
- size
- even, odd : 짝수, 홀수 번째 여부(1부터 카운팅), 결과는 boolean 값
- first, last : 첫번째, 마지막 여부, 결과는 booelan 값
- currnet : 컬렉션의 요소 중 현재 객체

예시
```html
<!-- menuStat이 '변수명 + Stat' 형태이기 때문에 menuStat 생략 가능-->
<tr th:each="menu menuStat : ${menus}">
    <td th:text=${menuStat.index}></td> -- index 출력
    <td th:text="${menu.name}"></td>
    <td th:text="${menu.price}"></td>
  </tr>
```
실행 결과
|번호|메뉴|가격|
|---|---|---|
|0|불고기|8000|
|1|삼겹살|12000|
|2|목살|10000|
