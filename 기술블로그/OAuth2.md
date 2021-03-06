# 들어가기

  안녕하세요,  유어슈에서 Back-end 개발을 리드하고 있는 아라입니다. 이 게시물들은 OAuth2 에 대한 개념을 알아보고 그에 대한 구현을 Node.js 로 해볼 예정입니다. 질문과 피드백은 언제나 환영합니다.



# 둘러보기

OAuth란?





# OAuth2

 OAuth 2.0은 인증을 위한 산업 표준 프로토콜입니다. OAuth 2.0은 다양한 어플리케이션 대한 특정 권한 부여 플로우를 제공하면서 클라이언트 개발자의 단순성에 중점을 두고 설계되었습니다. Google, Twitter, Github 등 대부분의 소셜 인증 기능을 지닌 제공자들은 표준 인증 방식으로 OAuth2를 채택하고 있으며 그에 따른 OAuth2 API들을 제공하고 있습니다.



# OAuth2의 역할

OAuth2는 다음과 같은 네가지 역할을 정의합니다.

## 리소스 소유자 (Resource owner)

- 보호된 자원에 대한 접근 권한을 부여할 수 있는 객체.
- 자원 소유자가 개인인 경우 자원 소유자는 최종 사용자가 됩니다.

## 리소스 서버 (Resource server)

- 보호될 수 있는 자원을 호스팅하는 서버입니다.

## 고객 (Client)

- 리소스 관리자의 보호된 자원과 인증에 대해 요청하는 어플리케이션입니다.
- 특정 구현 특성을 암시하지는 않으며 다양한 방법으로 구현될 수 있습니다.

## 인증 서버 (Authorization server)

- 고객에게 보호된 자원에 접근할 수 있는 토큰을 발행해줍니다.
- 리소스 소유자의 인증 및 권한을 부여해준다.



# OAuth2 플로우

OAuth2의 추상적인 플로우를 살펴보면 다음과 같다.



# 참고

https://oauth.net