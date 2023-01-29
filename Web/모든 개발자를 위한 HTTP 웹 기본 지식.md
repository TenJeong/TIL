# 모든 개발자를 위한 HTTP 웹 기본 지식

> ### HTTP 웹의 기본적인 내용을 정리하였습니다. 
>
> 해당 문서는 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://inf.run/HKd6)을 수강하면서 작성되었으며, [자주하는 질문](https://docs.google.com/document/d/1j0jcJ9EoXMGzwAA2H0b9TOvRtpwlxI5Dtn3sRtuXQas/edit?usp=sharing)에 기술되어 있는 규칙에 따라 학습 후의 내용을 필자의 생각으로 정리하여 수록됩니다. 또한 해당 강의는 유료 강의이며 강의 제공 측의 금지 사항으로 전체 소스 코드는 제공되지 않습니다.
>
> 이하의 내용들은 좀 더 간결하고 효율적으로 정리하기 위해 높임말 없이 작성되었습니다.

----------

## 목차

1. [URI와 웹 브라우저](#1. URI와-웹-브라우저)
2. [HTTP란?](#2. HTTP란?)

----------

## 1. URI와 웹 브라우저

#### URI: Uniform Resource Identifier

 리소스를 식별하기 위해 사용되는 통일된 방식을 의미하며, 리소스는 URI를 통해 식별될 수 있는 모든 것을 의미한다. 현재 보편적으로 사용되는 하위 개념으로는 URL과 URN 등이 있다.

![URI 형식](../resources/uri_format.png)

*출처: [RFC3986](https://www.rfc-editor.org/rfc/rfc3986#section-3)*

#### URL : Uniform Resource Locator

 리소스의 위치를 지정하는 규약이며, 흔히 말하는 웹 페이지의 주소 뿐만 아니라 컴퓨터 네트워크상의 모든 자원을 나타낼 수 있다.

![URL 형식](../resources/url_format.png)

![HTTP URL 형식](../resources/http_url_format.png)

*출처: [위키피디아 URL](https://ko.wikipedia.org/wiki/URL)*

###### Components

- Scheme: 주로 프로토콜을 사용한다. (e.g. http, https ftp 등)
- Userinfo: URL에 사용자정보를 포함해서 인증할 때 사용하지만 거의 사용하지 않는다.
- Host: 호스트명을 사용하며 도메인명이나 IP주소를 직접 사용할 수 있다.
- Port: 접속에 사용할 포트 번호이며, 프로토콜에 맞춰서 생략하는 경우가 많다.
- Path: 리소스의 경로를 사용하며, 계층적인 구조로 표현한다.
- Query: "Key" = "Value" 형태이며, '?'로 시작하고 '&'로 나열한다. Query parameter, Query string등으로 부른다.
- Fragment: 서버에 전송하지 않는 html 내부 북마크 등에 사용되는 정보이며, '#'로 시작한다.

#### URN: Uniform Resource Name

 리소스에 이름을 부여하는 규약이며, 위치와 달리 영속적인 특성을 가진다. URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않았기 때문에 상대적으로 사용되는 빈도 수는 적다.

![URN 형식](../resources/urn_format.png)

*출처: [위키피디아 URN](https://ko.wikipedia.org/wiki/URN)*

#### 웹 브라우저 요청 흐름

1. 클라이언트의 웹 브라우저가 HTTP 메시지를 생성한다.
2. 웹 브라우저의 Socket 라이브러리를 사용해서 도착지점과 TCP/IP 연결을 하고 OS에 데이터를 넘긴다.
   - IP와 Port 번호를 기본적으로 사용해서 도착지점을 파악하며, IP가 없고 도메인 이름이 있다면 DNS 조회로 IP를 알아내고, Port 번호가 없으면 프로토콜을 통해 포트를 유추한다.
3. HTTP를 포함한 TCP/IP 패킷을 생성하고 서버로 전송한다.
4. 서버는 패킷을 수신하고 이에 맞는 응답 메시지를 클라이언트에 전송한다.
5. 전송 받은 응답 메시지를 사용해서 웹 브라우저가 HTML 렌더링을 수행해서 화면에 보여준다.

----------

## 2. HTTP란?

#### HTTP: HyperText Transfer Protocol

 기본적으로 HTML 메시지와 Text 전송을 기반으로 했으며, 현재는 이미지, 음성, 영상, 파일 등의 거의 모든 형태의 데이터를 전송하는 것이 가능해졌다. 서버간의 데이터 송수신에서도 대부분 HTTP를 사용한다.

HTTP/1.1에서 현재의 기능을 거의 대부분 갖춰서 이를 주로 사용하고 있으며, HTTP/2와 HTTP/3의 사용 비율도 점차 증가하고 있다.

#### HTTP 특징

1. 클라이언트 - 서버의 구조에서 사용한다.
2. 무상태 프로토콜 (Stateless), 비연결성 (Connectionless)을 갖추었다.
3. HTTP 메시지를 통해 통신한다.
4. 단순하며 확장이 가능하다.

#### 무상태 프로토콜 (Stateless)

- 서버가 클라이언트의 상태를 보존하지 않는다.
- 장점: 서버를 확장할 때 용이하다.
- 단점: 클라이언트가 전송시 마다 추가 데이터를 전송할 필요가 있다.

#### Stateful, Stateless

- Stateful: 요청을 수행하는 서버가 각 클라이언트의 상태 정보를 저장하고 있는 방식이다.
  * 요청을 수행하는 도중에 처리하는 서버가 바뀌어선 안된다.
  * 장애 발생시에 처음부터 통신을 시작하거나, 상태정보를 다른 서버에 전송하는 등이 방식이 필요하다. 
- Stateless: 요청을 수행하는 서버가 클라이언트 정보를 저장하지 않는 방식이다.
  - 각 메시지 전송에 클라이언트의 상태를 실어서 전송한다.
  - 요청을 수행하는 도중에 처리하는 서버가 바뀌더라도 괜찮다.
  - 트래픽이 증가했을 때 서버 증설이 용이하다.

#### Stateless의 한계

 무상태로 설계하는 것에는 한계가 존재한다. 예를 들어 로그인 기능의 경우, 로그인한 사용자의 로그인 여부를 서버가 가지고 있을 필요가 있다. 일반적으로 이것은 브라우저 쿠키와 서버 세션을 함께 활용하는 등의 방법을 통해 구현한다. 꼭 상태 유지를 해야한다면 최소한의 상태만 유지할 수 있도록 한다.

#### 비 연결성 (Connectionless)

- 연결을 유지하는 모델: 한 번 이어진 연결을 계속해서 유지하며 이후 빠른 통신이 가능하지만 서버 자원을 소모한다.
- 연결을 유지하지 않는 모델: 요청 처리가 완료되면 해당 연결을 끊고 다른 요청시에 다시 연결하여 최소한의 자원을 유지한다.

HTTP는 기본적으로 Connectionless이며, 보통 매우 많은 사용자가 서비스에 요청해서 처리하는 양은 매우 작다고 할 수 있으므로 비 연결성을 갖추면 서버 자원을 효율적으로 사용할 수 있다.

HTTP 초기에는 각 요청 및 응답 마다 연결 만들고 끊어주는 것을 반복했지만 매우 비효율적이었고, 현재는 어느정도의 연결 유지를 통해 효율적으로 요청 처리를 수행할 수 있게 되었다.

최대한 무상태로 설계해야 특정 시간 대에 사용자가 몰리는 서비스를 서버 추가 등을 통해 간단하게 처리하는 것이 가능해진다.

#### HTTP 메시지

HTTP 메시지는 요청, 응답에 관계없이 다음과 같은 형태를 가진다. 각 라인들은 CRLF(개행문자)로 구분되지만, 보통 편의를 위해 CRLF를 생략하고 다른 줄에 적어서 시각적으로 구분한다.

- Start-line
- Header
- Empty-line
- Message body

`이하 HTTP 메시지 설명에서 각각 SP - 공백, CRLF - 개행, OWS - 띄어쓰기 허용을 의미한다.`

#### Start-line

##### (요청): request-line (응답): status-line

- request-line = **method** SP **request-target** SP **HTTP-version** CRLF
  - method(HTTP 메소드)
    - 서버가 수행할 동작을 지정한다.
    - GET, POST, PUT, DELETE 등이 있다.
  - Request-target(요청 대상)
    - **absolute-path[?query]** - **절대경로[?쿼리]** 형식을 취한다.
- status-line = **HTTP-version** SP **status-code** SP **reason-phrase** CRLF
  - status-code(HTTP 상태 코드)
    - 요청 수행의 성공 여부를 나타내는 코드이다.
    - 200(성공), 400(클라이언트 요청 오류), 500(서버 내부 오류) 등이 있다.
  - reason-phrase(이유 문구)
    - 결과에 대한 설명을 사람이 이해할 수 있는 짧은 문장 또는 단어로 나타냄

#### Header

- Header-field = **field-name**: OWS **field-value** OWS
  - 참고: field-name은 대소문자 구분이 없다.
- HTTP 전송에 필요한 모든 부가정보를 표현한다.

#### Message body

 실제 전송할 데이터로서 HTML 문서, 이미지, 영상, JSON등 byte로 표현할 수 있는 모든 데이터를 담아서 전송할 수 있다.