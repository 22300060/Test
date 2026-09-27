# HTML Form 실습

HTML Form → Form Elements → CSS → Validation → JavaScript → GitHub → Vercel

- GitHub Repository: (본인 저장소 URL 입력)
- Deploy URL: (Vercel 배포 URL 입력)
- Clone Coding 원본 URL: https://getbootstrap.com/docs/5.2/examples/checkout/

## 파일 구성

| 파일 | 내용 |
| --- | --- |
| `index.html` | 세 페이지로 가는 링크 목록 |
| `form1.html` | Form 태그 연습 + Bootstrap Checkout Clone Coding (CSS 없음) |
| `form1_css.html` | form1.html에 CSS 적용 |
| `form1_js.html` | form1_css.html에 HTML Validation + JavaScript 적용 |

---

## Weekly Review

### 1. Key Learning

1. **Form 구조**: `<form>` 안에 `<label>` + 입력 요소를 두고, `label`의 `for`와 입력 요소의 `id`를 같게 하면 둘이 연결된다. `fieldset`/`legend`로 관련 항목을 묶을 수 있다.
2. **CSS로 Form 꾸미기**: `display: block`, `width: 100%`, `padding`, `border-radius`로 입력칸을 정리하고 `:hover`, `:focus` 같은 상태 선택자로 사용자의 동작에 반응하게 만든다.
3. **Validation + JavaScript**: `required`, `type="email"`, `minlength`, `pattern` 같은 HTML 속성으로 규칙을 정하고, JavaScript의 `checkValidity()`로 그 규칙을 통과했는지 확인한다.

### 2. Form Elements

| 요소 | 사용한 곳 | 용도 |
| --- | --- | --- |
| `input type="text"` | 이름, Username, 주소, 우편번호, 카드 정보 | 한 줄 글자 입력 |
| `input type="email"` | Email | 이메일 형식 입력 (@ 포함 여부 검사) |
| `input type="password"` | Password | 입력한 글자가 가려짐 |
| `input type="radio"` | 결제 수단 | 같은 `name` 중 하나만 선택 |
| `input type="checkbox"` | 배송지 동일, 정보 저장, 약관 동의 | 여러 개 선택 / 켜고 끄기 |
| `input type="date"` | 희망 배송일 | 달력에서 날짜 선택 |
| `input type="month"` | 카드 만료일 | 연/월 선택 |
| `input type="color"` | 포장지 색상 | 색상 선택 |
| `select` + `optgroup` | Country | 목록에서 선택, 대륙별로 그룹 묶기 |
| `datalist` | City | 입력 시 추천 목록 표시 (직접 입력도 가능) |
| `textarea` | 배송 메모 | 여러 줄 입력 |
| `fieldset` + `legend` | 각 구역 | 관련 입력 묶기 + 제목 |
| `button` | 제출 / 다시 입력 | `type="submit"` 제출, `type="reset"` 초기화 |

### 3. HTML vs CSS (form1.html vs form1_css.html)

- **form1.html**: CSS가 없어서 브라우저 기본 모양. label과 input이 한 줄에 붙어 있고, 줄바꿈은 `<br>`로 처리했다. 입력칸 크기가 제각각이다.
- **form1_css.html**: HTML 구조는 같고, `<br>` 대신 `<div class="field">`로 한 줄씩 감쌌다.
  - `form`: 흰 배경, 테두리, `border-radius`, `max-width` + `margin: auto`로 가운데 정렬
  - `label`: `display: block`으로 입력칸 위에 배치
  - `input`/`select`/`textarea`: `width: 100%`, `padding`, `border`, `border-radius`로 크기 통일
  - `:hover`: 테두리 색 변경 / `:focus`: 파란 테두리 + 그림자
  - `button`: 배경색, 글자색, `cursor: pointer`, hover 시 더 진한 색

### 4. Validation & JS

**HTML Validation (form1_js.html)**

| 조건 | 적용 항목 |
| --- | --- |
| `required` | First name, Last name, Username, Email, Password, Address, Country, 희망 배송일, 약관 동의 |
| `type="email"` | Email |
| `minlength` | Username(3), Password(6) |
| `pattern` | Zip(`[0-9]{5}`), CVV(`[0-9]{3}`) |
| `maxlength` | 배송 메모(100) |

**JavaScript 처리 과정**

1. `document.querySelector("#userForm")`로 form을 찾는다.
2. `form.addEventListener("submit", function (event) { ... })`로 제출 이벤트를 처리한다.
3. `event.preventDefault()`로 페이지가 새로고침되는 기본 제출 동작을 막는다.
4. 검사할 입력 요소들을 순서대로 돌면서 `checkValidity()`로 확인한다.
5. 틀린 요소가 있으면 `alert()`로 메시지를 보여주고, `focus()`로 그 칸에 커서를 옮긴 뒤 `return`으로 함수를 끝낸다.
6. 모두 통과하면 `alert("... 등록이 완료되었습니다.")`를 띄우고 `form.reset()`으로 초기화한다.

**동작 확인 예시**

| 입력 | 결과 |
| --- | --- |
| 아무것도 입력하지 않고 제출 | "이름을 입력하세요." → First name에 focus |
| Email에 `abc` 입력 | "유효한 이메일을 입력하세요." → Email에 focus |
| Password에 `123` 입력 | "비밀번호는 6자 이상 입력하세요." |
| Zip에 `12ab` 입력 | "우편번호는 숫자 5자리로 입력하세요." |
| 모든 필수 항목을 올바르게 입력 | "OOO님, 등록이 완료되었습니다." |

### 5. Problem & Solution

- **문제 1**: form에 `required`를 넣었더니, 값이 틀리면 JavaScript의 submit 코드가 아예 실행되지 않았다.
  - **원인**: 브라우저가 먼저 검사해서 틀리면 submit 이벤트를 발생시키지 않는다.
  - **해결**: `<form novalidate>`를 추가해 브라우저 자동 검사를 끄고, JavaScript에서 `checkValidity()`로 직접 검사했다. (HTML 속성 규칙은 그대로 사용된다.)
- **문제 2**: `:invalid` CSS를 적용했더니 페이지를 열자마자 모든 필수 칸이 빨간색이 되었다.
  - **해결**: 제출 버튼을 누른 뒤에만 form에 `was-validated` 클래스를 붙이고, `.was-validated input:invalid`일 때만 빨간색이 보이도록 했다.
- **문제 3**: `select`에 `required`를 넣었는데 "Choose..."가 선택된 상태로도 통과되었다.
  - **해결**: 첫 번째 옵션의 값을 `value=""`로 비워 두면 선택하지 않은 것으로 처리된다.

### 6. Reflection

- `label`의 `for`와 `id`를 연결하면 글자를 눌러도 체크박스가 선택되는 것을 처음 알았다.
- `datalist`는 `select`와 비슷해 보이지만 목록에 없는 값도 입력할 수 있다는 점이 다르다.
- 궁금한 점: 브라우저 검사(HTML Validation)는 사용자가 개발자 도구로 속성을 지우면 우회할 수 있을 텐데, 실제 서비스에서는 서버에서도 다시 검사해야 하는지 궁금하다.

---

## Weekly Question (예시)

**Q1. (객관식)** 다음 중 `<form>`의 submit 이벤트에서 페이지가 새로고침되는 기본 동작을 막는 코드는?

1. `event.stopPropagation()`
2. `event.preventDefault()`
3. `form.checkValidity()`
4. `input.focus()`

- 정답: 2
- 해설: `preventDefault()`는 브라우저의 기본 동작(폼 제출 후 페이지 이동/새로고침)을 막는다. `stopPropagation()`은 이벤트가 부모 요소로 전달되는 것을 막는 것이고, `checkValidity()`는 유효성 검사, `focus()`는 커서 이동이다.

**Q2. (OX)** `<input type="radio">` 여러 개가 서로 다른 `name` 값을 가지고 있어도 그중 하나만 선택된다.

- 정답: X
- 해설: 라디오 버튼은 **같은 `name`** 을 가진 것끼리 하나의 그룹이 되어 하나만 선택된다. `name`이 다르면 서로 다른 그룹이라 모두 선택할 수 있다.
