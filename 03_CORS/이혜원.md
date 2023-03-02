# 동일 출처 정책 (Same Origin Policy, SOP)
- **같은 출처**를 가진 경우에만 리소스에 접근할 수 있게 하는 정책 -> 보안을 강화하기 위함
- **프로토콜**, **호스트**, **포트 번호**가 모두 같아야 같은 출처 (셋 중에 하나라도 다르면 다른 출처)
- 다른 출처에서 데이터를 읽어들이는 것을 방지하기 때문에 자바스크립트 코드로 다른 출처의 데이터에 접근할 수 없음 (`iframe`이나 `img` 등을 가져와서 사용하는 것은 허용)
- 다른 출처로의 링크, 리다이렉트, submit은 가능
- Internet Explorer의 경우, 포트 번호를 검사하지 않으며 양쪽 사이트가 모두 신뢰할 수 있는 사이트라면 리소스 접근 허용 (**비표준**)

# 교차 출처 리소스 공유 (Cross-Origin Resource Sharing, CORS)
- 현재 출처가 아닌 **다른 외부 출처**에 리소스 접근 요청을 보내기 위해 지켜야 하는 브라우저 정책
- **브라우저-서버** 간의 전송 시의 보안을 위한 기능 (서버-서버 간의 전송은 해당 없음)
- 기본적으로는 자격 증명 없이 이루어짐
- 브라우저는 요청 시 `Origin` 헤더에 자신의 출처를 전송
- 서버는 응답 시 `Access-Control-Allow-Origin` 헤더에 **허용되는 출처 목록**을 전송 -> 헤더에 포함된 출처만 리소스 접근 요청 가능
- 서버의 `Access-Control-Allow-Origin` 헤더의 값이 **\*** (**와일드카드**) 라면 **모든 출처**에서 오는 리소스 접근 요청을 허용하겠다는 의미
- 브라우저는 서버로부터 받은 헤더에 자신의 출처가 포함되는지 검사

## 안전한 요청
- 데이터를 바꾸지 않는 **안전한 메서드**를 사용하는 경우
    - `GET`
    - `POST`
    - `HEAD`

- **CORS-safelisted request header에 명시된 헤더**를 사용하는 경우
    - `Accept`
    - `Accept-Language`
    - `Content-Language`
    - `Content-Type` (값이 `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain` 중 하나여야 함)

### 단순 요청 (Simple Request)
- 안전한 요청 시, 프리플라이트 요청을 생략하고 서버에 본래 목적의 요청을 바로 보낼 수 있음

### 인증된 요청 (Credentialed Request)
- 브라우저의 요청은 기본적으로 다른 출처일 때는 쿠키 등의 인증 정보를 포함하지 않음
- 요청 시 `credentials: "include"` 옵션을 추가하면 어떤 출처라도 인증 정보를 포함하여 요청을 보내겠다는 의미
- 서버 응답의 `Access-Control-Allow-Origin` 헤더의 값이 \* 여서는 안 되고 출처가 정확히 명시되어 있어야 함
- 서버 응답의 `Access-Control-Allow-Credentials` 헤더의 값이 `true`여야 함

## 안전하지 않은 요청
- 안전한 메서드를 제외한 기타 HTTP 메서드(`PUT`, `PATCH`, `DELETE` 등)를 사용하는 경우
- CORS-safelisted request header에 명시되지 않은 기타 헤더를 사용하는 경우

### 프리플라이트 요청 (Preflight Request)
- 서버가 안전하지 않은 요청도 받을 수 있는지 확인하기 위해 미리 보내는 예비 요청
- 직접 작성할 필요 없이 필요한 경우 자동으로 전송됨
- 브라우저는 요청 시 `OPTIONS` 메서드를 사용
- `Access-Control-Request-Method` /  `Access-Control-Request-Headers` 헤더에 본래 목적의 요청에 사용할 메서드/헤더를 각각 적어 `Origin` 헤더와 함께 전송
- 서버가 안전하지 않은 요청을 허용하면, `Access-Control-Request-Method` / `Access-Control-Request-Headers` 헤더에 사용 가능한 메서드/헤더 목록을, `Access-Control-Max-Age` 헤더에 프리플라이트 요청의 캐싱 제한 시간을 응답으로 전송
- 응답으로 온 출처와 현재 출처가 같다면 브라우저는 본래 목적의 요청 전송
- 서버가 안전하지 않은 요청을 거부하면, 브라우저는 본래 목적의 요청을 보내지 않음

## 프록시 서버 (Proxy Server)
- CORS 허용은 서버 측에서 해 줘야 함 -> 그러나 서버 접근 권한이 없는 경우 **클라이언트 측**에서 사용 가능한 방법
- 브라우저와 서버의 **중간**에서 리소스 접근을 허용/거부하는 역할
- 브라우저는 프록시 서버에 요청을 보내고, 프록시 서버가 요청을 서버에게 전달 (서버-서버 간에는 CORS가 적용되지 않기 때문에 프록시 서버와 서버 사이의 통신에선 오류가 나지 않음)
- 서버가 응답하면 프록시 서버가 응답을 받아 브라우저에게 전달

## 참고 사이트
- https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy
- https://web.dev/same-origin-policy/
- https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
- https://developer.mozilla.org/en-US/docs/Glossary/CORS-safelisted_request_header
- https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request
- https://ko.javascript.info/fetch-crossorigin
- https://ui.toast.com/weekly-pick/ko_20211110
- https://react.vlpt.us/redux-middleware/09-cors-and-proxy.html