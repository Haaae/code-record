<details>
<summary>웹 통신의 큰 흐름: 브라우저 창에 https://www.google.com/ 를 입력하고 엔터를 눌렀을 때 벌어지는 일</summary>
<div markdown="1">

[참고](https://www.youtube.com/watch?v=GAyZ_QgYYYo&ab_channel=%EB%84%90%EB%84%90%ED%95%9C%EA%B0%9C%EB%B0%9C%EC%9E%90TV)

![ex_brawser](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FblP6Co%2FbtruDDt3Rwm%2FaxKes9njvknBskcmYCatuK%2Fimg.png)

- 브라우저가 URL을 분석한다.
- 분석에 따른 IP 주소를 가져온다.
    1. 컴퓨터의 hosts 파일을 통해 IP 주소를 가져온다.
        
        > hosts 파일이란 IP 주소와 도메인 주소를 매핑해주는 파일이다.
        >
        > 보통, 주소창에 도메인으로 접속하면 DNS 서버를 통해 이에 대응하는 IP 주소를 찾아서 서버에 접속하게 된다. 이때 hosts 파일에 도메인과 IP를 임의로 지정하게 되면 DNS 서버보다 우선된다.

        > 장점
        > - 인터넷 속도 향상
        > - 리소스 사용 감소
        > - 보안 문제에 대한 예방

        > 단점
        > - 사이트 방문이 차단될 수 있음
        > - 페이지 내에서 부분 차단된 경우 디자인, 속도 이슈 발생 가능

    2. DNS Cache에서 찾는다.

        > 브라우저의 캐시에는 한 번 질의했던 DNS 기록들이 저장되어 있다. URL과 같은 호스트 네임 등의 문자를 IP 주소로 변경해주는 도메인 네임 서버(DNS)에서 한 번 질의된 도메인 네임과 해당 IP 주소를 캐시에 TTL(Time To Live)만큼 유지하여 같은 질의가 올 때 캐시에서 응답해주는 것이다.

    3. OS 캐시, router 캐시, ISP 캐시 순서대로 최대한 빠르게 DNS 기록을 찾는다.

    4. 캐시 서베에서 DNS 기록을 찾지 못하면, 로컬 DNS 서버에서 URL 주소에 해당하는 IP 주소를 DNS에 질의한다. 이때 방식은 PC 네트워크 설정에 따라 나뉠 수 있다.
        
        > 1. 가령 DNS가 공유기로 지정되어 있는 경우 공유기가 DNS 포워딩 기능을 통해 DNS에 질의한다.
        > 
        > 2. 혹은 DNS에 직접 질의할 수도 있다.    

        > 도메인 네임 소유자 입장에서는, 이때 DDNS를 사용할 수 있다. 


- ARP 프로토콜을 통해 논리적인 주소인 IP 주소를 물리 주소인 MAC 주소로 변환한다.
    
    > - 브로드캐스트를 통해 ARP 요청 패킷을 전송하여 해당 IP 주소(L3)를 갖고 있는 노드가 자신의 MAC 주소(L2)를 알려준다.
    > - 그 이유는 IP 주소는 고유한 주소가 아니고, Dynamic Host Configuration Protocol에 의해 동적으로 할당된 논리적인 주소이기 때문이다. 
    > - 따라서 네트워크 인터페이스 카드의 고유한 번호 MAC 주소를 알아야 한다.


- MAC 주소를 이용해 알아낸 호스트와 TCP 소켓을 연결한다. 이때 3-way handshake를  사용한다.

    - 클라이언트가 SYN 패킷을 전송 → 서버가 SYN, ACK 패킷 응답 → 클라이언트가 ACK 패킷을 전송
    - 만약 HTTPS 요청이라면 TLS handshake 과정이 추가된다.


- TCP 연결이 확정되었다면 클라이언트는 HTTP 프로토콜을 이용하여 서버에게 요청한다. (URL 입력이라면 GET 요청..)


- 서버는 요청이 무엇인지 파악하고 HTTP 프로토콜을 이용해 응답한다.


- 브라우저가 HTML 컨텐츠를 렌더링해서 보여준다.

---
만약 도메인 소유자가 GSLB(Global Server Load Balancing, 전역 서버 부하 분산) 방식을 사용한다면 상술한 내용 중 DNS에게서 IP 주소를 가져오는 과정은 변화할 수 있다.

1. DNS에게서 도메인에 대한 IP 주소를 가져오는 것이 아닌, GSLB의 IP 주소를 가져온다.
2. GSLB에게 도메인에 대한 IP 주소를 질의하면 GSLB는 헬스체크를 통해 사용가능한 IP 주소 중 특정 알고리즘에 의한 IP 주소를 반환한다.
3. LDNS 는 GSLB 로부터 web.google.com 의 IP 주소를 받고, 캐싱을 한 뒤, 클라이언트에게 전달한다. 
4. 클라이언트는 최종 응답받은 IP 주소로 패킷을 보낸다. 

- GSLB의 사용목적
    - 서버의 상태를 고려하여 IP를 제공할 수 있다. 덕분에 재해 복구나 사이트의 부하 분산을 꾀할 수 있다.

```
GSLB와 CDN: CDN은 지역적 분산을 통해 사용자 응답 속도 향상을 꾀하는 기술이고, GSLB는 특정 사용자의 IP 질의에 대해 여러 서버 IP 중 최적의 IP를 제공하는 기술이다. 따라서 CDN을 구현하는 과정에서, Server를 지정할 때 GSLB가 기존 DNS의 역할을 대신한다고 볼 수 있다.
```

</div>
</details>


<details>
<summary>URL과 URI</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>TCP와 UDP</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>TCP 3, 4way hankshake란?</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>OSI7계층과 TCP/IP 4계층</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>웹 서버 소프트웨어(Apache, Nginx)는 OSI 7계층 중 어디서 작동하는가</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>웹 서버 소프트웨어(Apache, Nginx)의 서버 간 라우팅 기능은 OSI 7계층 중 어디서 작동하는가</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>Proxy Server</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>로드밸런싱</summary>
<div markdown="1">

- 만약 여러 분산 서버 중 한 곳에 로그인했는데 그 서버가 장애가 발생한 경우, 로그인이 유지되게 하는 방법은?

</div>
</details>


<details>
<summary>DNS</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>REST와 RESTful API</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>CORS</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>쿠키(Cookie)와 세션(Session)</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>세션(Session)과 토큰(Token)</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>JWT 토큰</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>BCrypt와 그 외</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>대칭키, 비대칭키 암호화 방식</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>Connection Timeout과 Read Timeout</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>공인(public) IP와 사설(private) IP</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>HTTP 프로토콜</summary>
<div markdown="1">

추가예정

</div>
</details>

<details>
<summary>HTTP 메서드의 종류와 그 의미</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>SSL과 HTTPS</summary>
<div markdown="1">

추가예정

</div>
</details>