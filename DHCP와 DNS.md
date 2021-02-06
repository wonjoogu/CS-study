# 🔅 DHCP
## ◽ 주요 구성 및 동적 할당
  * Dynamic Host Configuration Protocol(동적 호스트 설정 프로토콜)의 약자
  * 해당 호스트에게 **IP주소, 서브넷 마스크, 기본 게이트웨이 IP주소, DNS 서버** IP 주소를 자동으로 일정 시간 할당해주는 인터넷 프로토콜
  * 동적 할당 : 제한된 수량의 IP주소를 재사용, 한시적 사용, 자동 재활용 기능
  * 디스크 없는 컴퓨터에게 IP 주소 등 기타 구성 정보를 제공하는 기능
  
     <p align="center"><img src="https://www.netmanias.com/ko/?m=attach&no=1996" width="80%" height="50%"></img></p>
  
## ◽  특징
  * **클라이언트 / 서버 형태**의 동작
    * 동적인 구성 정보를 요청 / 제공하는 프로토콜
    * DHCP 클라이언트(요청) 및 서버(응답)가 동일 서브넷에 함께 있을 수도, 다른 망에 분리될 수도 있음(분리되는 경우 DHCP 중계 에이전트가 작동)
    
  * 프로토콜 및 포트
    * 수송용(Transport Layer) 프로토콜 : UDP
    * 사용 포트
      * DHCPv4 : 67 (서버용), 68 (클라이언트용)
      * DHCPv6 : 546 (서버 송출, 클라이언트 청취), 547 (서버 청취, 클라이언트 송출)
      
  * DHCP 탐색(Discover) / 요청(Request) 때 쓰이는 IP헤더 내 IP주소
    * IP헤더 내 발신지 주소 : 0.0.0.0
    * IP헤더 내 목적지 주소
      * DHCPv4 : 255.255.255.255(UDP 브로드캐스트 주소)
      * DHCPv6 (DHCP에 대한 모든 참조를 나타냄) : ALL_DHCP_Relay_Agents_and_Servers(FFD2::1:2).ALL_DHCP_Server(FF05::1:3)
          - 요청 받는 서버에서는 요청 클라이언트의 MAC 주소를 기억하고, 이를 IP 주소와의 매핑시 이용
  
## ◽ DHCP 동작 원리
 *  DHCP로 IP주소를 할당할 때, DHCP 클라이언트와 DHCP 서버는 UDP(User Datagram Protocol)패킷으로 4가지 메시지를 주고 받는다.   
   DHCP 서버와 DHCP 클라이언트는 각각 UDP 포트 67과 68로 패킷을 수신

   메시지 이름 | 보낸 곳 | 설명
   -----------|---------|-------
   DHCP - DISCOVER | 클라이언트 | DHCP 서버를 찾아서 IP주소 할당을 요청 ( Client -> DHCP Server )
   DHCP - OFFER | 서버 | 할당 후보 IP주소를 제시 ( DHCP Server -> Client )
   DHCP - REQUEST | 클라이언트 | 후보로 제시된 IP 주소 사용을 요청, 혹은 IP 주소의 유효 기간 연장을 요청 ( Client -> DHCP Server )
   DHCP - ACK | 서버 | 클라이언트의 요청을 받아들임 ( DHCP Server -> Client )
   DHCP - RELEASE | 클라이언트 | IP 주소 해제 통지 ( Client -> DHCP Server )
   DHCP - NAK | 서버 | 요청 거부, 거부 메세지 ( DHCP Server -> Client )
   DHCP - INFORM | 클라이언트 | IP 주소 이외의 설정 정보 ( Client -> DHCP Server )

# 🔅 DNS
  * Domain Name System의 약자
  * **사용자가 입력한 도메인 주소를 IP 주소로 변환**하는 과정
  
## ◽ 주요 구성
 * 도메인 네임 공간 (Domain Name Space) - 저장 / 관리하는 계층적 구조, 거대한 분산 네이밍 시스템
 
 * 네임 서버 (Name Server) - 도메인 이름을 IP 주소로 변환하는 것을 네임 서비스라고 하며,   
 해석기로부터 요청 받은 도메인 이름에 대한 IP 정보를 다시 해석기로 전달해주는 역할을 수행
 
 * 해석기 (Resolver) - 웹 브라우저와 같은 DNS 클라이언트의 요청을 네임 서버로 전달하고 네임 서버로 부터 정보(도메인 이름과 IP 주소)를    받아 클라이언트에게 제공하는 기능   
 
 
## ◽ DNS 동작 원리
 
   사용자가 요청한 도메인의 **IP 주소**는 **Local DNS 서버가 다른 DNS 서버와 통신하여 사용자에게 제공.**   
   즉, Local DNS 서버는 사용자의 요청을 대신하여 해당 도메인의 IP 주소 정보를 획득할 때까지   
   필요에 따라 여러 DNS 서보와의 통신을 수행하고(Recursion, 재귀적 통신), 해당 정보를 획득한 후 사용자에게 그 결과 값을 전달  
   
   <p align="center"><img src="https://2.bp.blogspot.com/-xx-EcJfo8vM/W4bHCi5ikKI/AAAAAAAAAMI/-hdKk0o9Xd4HMcHbMA9IpmX8uSAgkAhmACLcBGAs/s1600/20.png" width="100%" height="50%"></img></p>
   
   1. PC에 미리 설정되어 있는 Local DNS에게 www.naver.com(Host Name) 이라는 hostname에 대한 IP주소 물어본다.
   2. 다른 DNS 서버와 통신 시작. 먼저 Root DNS 서버에게 물어본다.
   3. com을 관리하는 DNS 서버를 알려준다.
   4. Local DNS 서버가 com(*Top-level Domain Name) 관리하는 서버에게 www.naver.com 주소를 물어본다.
   5. naver.com(*Second-level Domain Name) 도메인을 관리하는 DNS 서버를 알려준다.
   6. Local DNS 서버가 naver.com 관리하는 서버에게 www.naver.com 주소를 물어본다.
   7. www.naver.com 에 대한 IP 주소를 응답해준다.
   8. Local DNS 에서는 www.naver.com 에 대한 IP 주소를 *캐싱하고 PC에게 IP 주소를 알려준다.   
   
   ❔ 캐싱이란 ? 
    - www.naver.com 에 담겨있는 호스트 네임 등의 문자를 IP 주소로 변경해주는 도메인 네임 서버(DNS)에서   
    한 번 질의된 도메인 네임과 해당 IP주소를 캐시에 TTL(컴퓨터 및 컴퓨터 네트워크 기술에서의 시간 또는 전송의 횟수)   
    만큼 유지하여 같은 질의가 올 때 캐시에서 응답해주는 것   
    
    ✔ 이와 같이 Local DNS 서버가 여러 DNS 서버를 차례대로 (Root DNS 서버 -> com DNS 서버 -> naver.com DNS 서버) 물어보고   
    답을 얻으러 찾는 과정을 Recursive Query라고 한다.
    
    ✔ Top-level Domain : Root Domain 바로 아래의 단계를 1단계 도메인 또는 최상위 도메인(TLD)라고 한다.
    ✔ Second-level Domain : 그 다음 단계를 2단계 도메인(SLD)이라고 한다.   
    
    
 ### 💫 면접에서 많이 나온 질문
 
 #### Q. 주소창에 naver.com을 치면 일어나는 일 
 
 ❕ 우선 IP 주소와 도메인 네임에 대한 지식이 필요하다.   
 
 1. IP 주소   
    * 컴퓨터가 인터넷 상에서 서로를 인식하기 위해 지정받은 식별용 번호
    * 현재는 IPv4 버전(32비트)로 구성   

 2. 도메인 네임 (Domain Name)
    * 12자리의 IP주소를 외우기 어렵기 때문에 문자로 표현한 주소
    * 의미있는 문자들과 . 의 조합
    * 사람의 편의성을 위해 만든 주소이므로 컴퓨터가 이해할 수 있는 IP 주소로 변환 작업 필요
    * 이때, 사용할 수 있도록 미리 도메인 네임과 함께 해당하는 IP 주소값을 한 쌍으로 저장하고 있는 데이터베이스를 DNS라고 한다.
    * 도메인 네임으로 입력하면 DNS를 이용해 컴퓨터는 IP 주소를 받아 찾아갈 수 있다.   
 
<p align="center"><img src="https://camo.githubusercontent.com/1d907c5b70e225e976ae41bb729c65e4c7e047a550aea90cd7fc7576aff72f32/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f393946303939333735433132344232443032" width="80%" height="50%"></img></p>

 A.   
 
  1. 사용자가 브라우저에 도메인 네임(www.naver.com)을 입력한다.
  
  2. 사용자가 입력한 URL 주소 중에서 도메인 네임(Domain Name) 부분을 DNS 서버에서 검색하고, DNS 서버에서 해당 도메인 네임에 해당하는 IP 주소를 찾아 사용자가 입력한 URL 정보와 함께 전달한다.
  
  3. 페이지 URL 정보와 전달받은 IP 주소는 HTTP 프로토콜을 사용하여 HTTP 요청 메시지를 생성하고, 이렇게 생성된 HTTP 요청 메시지는 TCP 프로토콜을 사용하여 인터넷을 거쳐 해당 IP 주소의 컴퓨터로 전송된다.
  
  4. 이렇게 도착한 HTTP 요청 메시지는 HTTP 프로토콜을 사용하여 웹 페이지 URL 정보로 변환되어 웹 페이지 URL 정보에 해당하는 데이터를 검색한다.
  
  5. 검색된 웹 페이지 데이터는 또 다시 HTTP 프로토콜을 사용하여 HTTP 응답 메시지를 생성하고 TCP 프로토콜을 사용하여 인터넷을 거쳐 원래 컴퓨터로 전송된다.
  
  6. 도착한 HTTP 응답 메시지는 HTTP 프로토콜을 사용하여 웹 페이지 데이터로 변환되어 웹 브라우저에 의해 출력되어 사용자가 볼 수 있게 된다.
  
    
    
