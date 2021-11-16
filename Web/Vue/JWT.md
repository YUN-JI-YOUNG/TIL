## Authentication & Authorization  

- Authentication   
  - 인증, 입증    
  - 자신이라고 주장하는 사용자가 누군지 확인하는 행위이며, 모든 보안 프로세스의 첫 단계   
  - 401 Unauthorized     
    - HTTP 표준에서는 미승인이라고 하지만, 의미상으론 비인증 의미      
- Authorization     
  - 권한 부여, 허가
  - 사용자에게 특정 리소스 또는 기능에 대한 액세스 권한 부여 과정(절차)   
  - 보안환경에서 권한부여는 항상 인증을 따라야 함   
  - 인증 되었더라도 모든 권한을 부여 받는 것은 아님 (`ex` 일반 유저 vs 관리자 유저)     
  - 403 Forbidden    
    - 401과 다른 점은 서버는 클라이언트가 누구인지 알고 있음      



- 세션(Session based), 토큰(Token based), 제 3자 활용(Authentication platform) 등 다양한 인증 방식 존재      
  - Django 는 세션 방식(Session based)   
    - Django http는 상태가 없고 비연결성의 특징을 갖고 있었으므로       



### JWT    

- 토큰 인증 방식 (Token based)     

- JSON Web Token    

- JSON 포맷을 활용하여 요소간 안전하게 정보를 교환하기 위한 표준 포맷     

- 암호화 알고리즘에 의한 디지털 서명이 되어있기 때문에 JWT 자체로 검증 가능하고 신뢰할 수 있는 정보 교환 체계    

- 필요한 정보를 모두 갖고 있어 이를 검증하기 위한 별도의 수단이 필요 없음     

  = `self-contained`        

  - DB를 사용하여 토큰의 유효성 검사를 할 필요 없다는 점이 세션이나 기본 토큰을 기반으로 한 인증과의 핵심 차이점    

- Authentication, Information Exchange .. 등에서 사용    

- 활용 이유   

  - 세션에 비해 상대적으로 HTML, HTTP 환경에서 사용하기 용이

    -> 세션 방식은 유저의 세션 정보를 서버에 보관해야 함   

    -> 하지만 JWT는 Client Side에 토큰 정보를 저장하고 필요한 요청에 유효 토큰을 같이 보내면 그 자체가 인증 수단

  - 자체의 보안 수준

    - 특정 요소 하나만 바뀌어도 모든 데이터가 변경되어 위/변조 불가

  - JSON의 범용성

  - 서버 메모리에 정보를 저장하지 않아도 되어 서버 자원 절약 가능 

- 구조 

  - `.` 연산자를 사용해 3 파트로 구분

    - Header 

      - 토큰의 유형(typ), 핵심 알고리즘(alg)으로 구성    

    - Payload  

      - 토큰에 넣을 정보  

      - claim 은 정보의 한 조각을 의미   

        -> Payload에는 여러개의 claim을 넣을 수 있음  

    - Signature

      - Header와 Payload의 인코딩 값을 더하고 거기에 private key로 해싱하여 생성

        `HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)`     

- 로그아웃 예시   
  - 로그아웃을 하더라도 유효 토큰이 어딘가 살아 있다면 누군가는 인증할 수 있으므로 최소한의 안전장치 설정     
    - 토큰의 합리적 만료 시간 설정  
    - 로그아웃 시 클라이언트에 저장된 토큰 삭제 (Brower Storage)
    - 블랙 리스트 설정 -> 더이상 유효하지 않거나 만료되지 않은 모든 토큰을 블랙리스트 처리   



|                      | Session          | JWT                   |
| -------------------- | ---------------- | --------------------- |
| 인증수단 저장위치    | Server           | Client                |
| 인증수단 정보 민감성 | Low (session id) | High (self-contained) |
| 유효 기간            | Y                | Y                     |
| 확장성               | 상대적 낮음      | 상대적 높음           |

- 두개를 적절하게 사용할 것    

