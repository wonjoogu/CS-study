# DHCP
* 주요 구성 및 동적 할당
  * Dynamic Host Configuration Protocol(동적 호스트 설정 프로토콜)의 약자
  * 해당 호스트에게 IP주소, 서브넷 마스크, 기본 게이트웨이 IP주소, DNS 서버 IP 주소를 자동으로 일정 시간 할당해주는 인터넷 프로토콜
  * 동적 할당 : 제한된 수량의 IP주소를 재사용, 한시적 사용, 자동 재활용 기능
  * 디스크 없는 컴퓨터에게 IP 주소 등 기타 구성 정보를 제공하는 기능
  
* 특징
  * 클라이언트 / 서버 형태의 동작
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
  
  * DHCP 동작 원리
      DHCP로 IP주소를 할당할 때, DHCP 클라이언트와 DHCP 서버는 UDP(User Datagram Protocol)패킷으로 4가지 메시지를 주고 받는다.   
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

# DNS
  * Domain Name System의 약자
  
  * 사용자가 입력한 도메인 주소를 IP 주소로 변환하는 과정
  
* 주요 구성
 * 도메인 네임 공간 (Domain Name Space) - 저장 / 관리하는 계층적 구조, 거대한 분산 네이밍 시스템
 
 * 네임 서버 (Name Server) - 도메인 이름을 IP 주소로 변환하는 것을 네임 서비스라고 하며,   
 해석기로부터 요청 받은 도메인 이름에 대한 IP 정보를 다시 해석기로 전달해주는 역할을 수행
 
 * 해석기 (Resolver) - 웹 브라우저와 같은 DNS 클라이언트의 요청을 네임 서버로 전달하고 네임 서버로 부터 정보(도메인 이름과 IP 주소)를    받아 클라이언트에게 제공하는 기능   
 
 
 * DNS 동작 원리
 
   사용자가 요청한 도메인의 IP 주소는 Local DNS 서버가 다른 DNS 서버와 통신하여 사용자에게 제공.   
   즉, Local DNS 서버는 사용자의 요청을 대신하여 해당 도메인의 IP 주소 정보를 획득할 때까지   
   필요에 따라 여러 DNS 서보와의 통신을 수행하고(Recursion, 재귀적 통신), 해당 정보를 획득한 후 사용자에게 그 결과 값을 전달  
   
   https://2.bp.blogspot.com/-xx-EcJfo8vM/W4bHCi5ikKI/AAAAAAAAAMI/-hdKk0o9Xd4HMcHbMA9IpmX8uSAgkAhmACLcBGAs/s1600/20.png
   
