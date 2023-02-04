
# 프로젝트 명칭

* 무선 와이파이 해킹 프로젝트
---

# 팀원 소개

![](https://velog.velcdn.com/images/yyyyyy/post/f330853e-a5d6-4be4-a0cb-a317b03e8929/image.png)

---

# 1. 서론

현재 현대인들의 필수품인 스마트폰 같은 이동 단말기 사용률이 증가하면서 사람들의 집에는 물론이고 카페, 공원, 도서관, 지하철 등의 공공장소에서 인터넷을 이용할 수 있게 하는 무료 와이파이를 쉽게 찾아볼 수 있다. 그러나 많은 사람들이 공공 무선 공유기의 와이파이는 보안에 취약할 수 있다는 것을 인지하지 못하고 사용하는 경우가 많다.

무선 공유기가 취약한 이유는 유선과 다르게 물리적인 거리의 제한이 없기 때문이다. 무선 인터넷을 사용하기 위해 무선랜의 전원을 킨다면 전파가 나오는데, 보통 이 전파는 100m 내외이다. 이렇게 되면 해킹을 시도하는 해커로부터 거리적인 보안을 하더라도 외부로부터 자신의 데이터를 100% 보호할 수 없다.

인증되지 않은 공공 와이파이는 해커에게 무방비로 노출되어 있을 확률이 크며, 그 와이파이를 사용한다면 해커에 의하여 해당 와이파이 사용자들의 금융이나 개인정보가 유출될 확률이 크다.

따라서 우리는 무선 네트워크의 취약점을 알고 공공 와이파이에 대한 보안 인식을 기르며, 경각심을 가지기 위해 이 프로젝트를 진행하게 되었다.

## 1-1. WEP
유선랜과 비슷한 수준의 데이터 보안성을 갖추기 위해 만들어진 무선랜의 표준 보안 방식 중 하나이다.

### 1) RC4
WEP이 사용하는 암호화 알고리즘이다. Client 단말기와 AP 간의 안전한 네트워크 사용을 위해 비밀 키를 생성하고 통신하는 데이터를 암호화하며 Stream 암호 방식을 사용한다.

 #### 1-1) 키 스트림(Key Stream)
Stream 암호 방식에서 생성되는 키(key) 값들의 배열이다. WEP에서의 키 스트림은 40비트 길이의 WEP 공유 비밀 키와 24비트의 초기화 벡터(IV)를 이용하여 생성된다.

 #### 1-2) 초기화 벡터(IV)
IV(Initialization Vector)라고도 불리며 Stream 암호 방식에서 사용하는 시드(Seed)값이다. 24비트이며 패킷마다 랜덤하게 생성된다. Stream 암호 방식의 키 스트림이 생성되기 위해서 필요하다.



### 2) WEP이 취약한 이유
 WEP이 취약한 가장 큰 이유는 키 스트림이 재사용되기 때문이다. 키 스트림이 재사용되는 이유를 알아보자.
 
해커가 2 개의 평문 (P1, P2)을 동일한 키 스트림(ks)에 의해 암호화 한 암호문 (C1, C2)를 가지고 있다고 하자. 암호문은 다음과 같이 생성된 것이다.

>C1 = P1 ⊕ ks
>C2 = P2 ⊕ ks


그러면 다음과 같은 식으로 2 개의 암호문을 서로 XOR 한 값은 2 개의 평문을 서로 XOR 한 값과 같다.

> C1 ⊕ C2 = ( P1 ⊕ ks ) ⊕ ( P2 ⊕ ks ) = P1 ⊕ P2

 이 수식을 기반으로 암호문(C1, C2)을 가지고 있는 해커가 평문 P1을 알고 있다고 가정하면, ks의 값을 알아낸 후 나머지 평문 P2도 획득할 수 있다.

> ( C1 ⊕ C2 ) ⊕ P1 = P2

 암호문(C1, C2)만 알고 있다고 가정해도 위의 식에 의하여 2 개의 암호문을 XOR 한 값이 2개의 평문을 XOR 한 값과 같으므로 평문 구조를 추측하여 원본 평문을 찾아낼 수 있다.


따라서 이러한 공격을 방지하기 위해 초기화 벡터가 고안된 것인데, 초기화 벡터에도 문제점이 있다. WEP에서 사용하는 초기화 벡터는 24 비트이며 랜덤하게 생성되는데, 그러면 통계학적으로 5,000개 정도의 패킷이 생성될 때 1번 재사용할 확률이 생긴다. 따라서 WEP의 암호화 알고리즘 설계상 1/5000 확률로 같은 초기화 벡터가 재사용된다. 만약 해커가 재사용되는 초기화 벡터를 찾았다면 위의 방법을 이용해 암호문을 복호화 할 수 있을 것이다.

 WEP이 취약한 또 다른 이유는 키의 길이 때문이다. WEP 키는 40 비트의 길이로 짧은 길이를 가지고 있어 짧은 시간 내에 공격에 성공할 수 있다. 이 점을 보완하기 위해 WEP 키 길이가 104 비트로 업데이트된 WEP2가 나왔지만, WEP2마저도 빠른 시간 내에 공격에 성공시킨 사례가 있다.



## 1-2. WPA
 TKIP 암호화 방식을 사용하는 무선랜의 표준 보안 방식 중 하나이다.

### 1) TKIP (Temporal Key Integrity Protocol)
 ‘임시 키 무결성 프로토콜’ 이라고도 불리며 WPA의 암호화 방식이다. WEP와 같이 RC4 암호화 알고리즘을 사용한다. 그러나 WEP에 존재했던 키 재사용 취약점을 보완하기 위해 초기화 벡터와 키에 보안 기술을 적용했다. WEP에서 24 비트였던 초기화 벡터를 48 비트로 확장시켰고, 고정되어 있던 키는 임시로 생성되어 통신 과정에서 자동으로 바뀌게 된다.



## 1-3. WPA2
  AES 암호화 방식을 사용하는 무선랜의 표준 보안 방식 중 하나이다.

### 1) AES (Advanced Encryption Standard)
 WPA2의 암호화 방식으로 암호화와 복호화를 할 때 동일한 키를 사용하는 대칭키 알고리즘이다. 암호화 키의 길이론 128, 192, 256비트를 사용하며 암호화된 데이터는 AES 복호화 키가 있어야 원본 데이터로 복호화가 가능하다. 현재 가장 보안성이 안전한 방식이다.

## 1-4. WPA/WPA2

### 1) PSK (Pre Shared Key)
무선 랜 인증 방식으로 WEP과 달리 WPA/WPA2에서 암호 키는 해커가 중간에 자료를 가로채더라도 해독하기 어렵게 하기 위해 통신 과정에서 자동으로 변경된다. 따라서 AP와 Client 단말기가 인증을 시도할 때 서로를 확인하기 위해 사용하는 사전에 미리 공유한 키를 PSK라 부른다.

### 2) 4-Way Handshaking

AP와 Client 단말기의 가상 회선 연결을 해제하기 위해 수행하는 확인 과정이다.
![](https://lh4.googleusercontent.com/CBw_0q9KuiF3mJFBdOO6w3PMTUkWGTtUNGCkOERaTU9pqjNE4WNeHAfcju2z_KN22iWaQUs6O8kUTgSAW23V6-ltXaCcWOl3Aidpa53icATI3NC_G72fU63z1tF4M8TvMlQ2RRM7kR1ctYIlC-cL8DdJzCiqlv0urpN8r0IwQJROTCcpHcciRBngudOZjA)
WPA/WPA2에는 현재까지 알려진 암호화 취약점이 없기 때문에 WPA/WPA2가 PSK 인증 방식일 때 4-Way Handshaking 과정에서 AP와 Client 단말기가 패스워드를 암호화한 형태의 패킷으로 공유한다는 점을 이용하여 패스워드를 알아낼 수 있다. 따라서 암호화된 패킷을 가로채서 사전 대입 공격(Dictionary attack)이나 무차별 대입 공격(Brute force)을 사용하면 평문 패스워드를 알아낼 수 있다.

---

# 2. 공격 시나리오

## 2-1. WEP key crack 공격 시나리오
WEP에 존재하는 키 재사용 취약점을 이용해 공격 시나리오를 세운다.

### 1) Reconnaissance
- 공격 대상이 될 주변의 AP 검색, 해당 AP에 대한 정보를 습득한다.

### 2) Packet Collection
 - 공격할 AP의 채널로 고정하고 IV를 수집하기 위한 파일을 만든다.
 
### 3) ARP Request Replay Attack
 -  ARP 요청 공격, AP가 패킷을 보낼 때 그 패킷을 똑같이 재전송하는 공격을 한다.
 
### 4) Deauthentication Attack
 - 인증 해제 공격, 클라이언트가 AP에 연결할 때 ARP 패킷을 보내는 점을 이용하여 의도적으로 AP와 클라이언트 간의 연결을 강제로 해제시킨다.

### 5) Crack
 -  수집한 IV로 WEP 크랙을 시도한다.


## 2-2. WPA/WPA2 key crack 공격 시나리오

4-Way Handshaking 과정에서 얻은 데이터를 이용해 시나리오를 세운다. 

### 1) Reconnaissance
 - 공격 대상이 될 주변의 AP 검색, 해당 AP에 대한 정보를 습득한다.

### 2) Packet Collection
 - 공격할 AP의 채널로 고정하고 4-Way Handshaking 과정 중 발생하는 패킷을 수집하기 위한 파일을 만든다.

### 3) Deauthentication Attack
 - 4-Way Handshaking 과정 중 발생하는 패킷을 수집하기 위해 기존에 연결된 단말기와 AP의 연결을 강제로 해제시킨다.

### 4) Crack - Dictionary Attack
 - 사전 파일을 만들어 Dictionary 공격으로 크랙을 시도한다.

--- 
# 3. 실습

위 실습은 목차 *2. 공격 시나리오* 를 바탕으로 시행한다. 실습은 총 두 가지로
 *2-1. WEP key crack* 과 *2-2. WPA/WPA2 key crack*으로 구성된다. (모든 실습은 root 권한으로 진행한다.)


## 3-1. 실습 환경 구성

 * 사용한 IoT장비
   *  ip Time N150UA 무선 랜 카드
 
* 사용 툴
  * 칼리 리눅스

## 3-2. 실습 - WEP key crack

처음에 '`iwconfig`' 명령어로 연결된 무선 랜 카드인 wlan0을 확인하면 Mode:Managed로 설정되어 있다. Managed 모드는 칼리 리눅스에 직접 연결된 패킷만 캡처한다.
범위 내에 있는 모든 패킷을 캡처하기 위해서 '`airmon-ng start wlan0`' 명령어를 입력해 Mode:Monitor(무차별 모드)로 전환한다.

>kali# airmon-ng start wlan0

![](https://velog.velcdn.com/images/yyyyyy/post/0b2a14d9-22be-48de-91d2-d457db97ee48/image.png)


### 1) Reconnaissance

패킷을 수집한 준비가 되었으면,' `airodump-ng wlan0`' 명령어를 입력하여 주변의 AP를 검색한다. 공격 대상이 될 AP를 찾아 BSSID(MAC 주소)와 CH(채널 번호)를 기억해둔다. 본 실습의 목표 대상은 WEP을 사용하는 실습용 Layer7 AP이다.

 >   kali# airodump-ng wlan0
    
   **![](https://lh6.googleusercontent.com/da7XZqF2yOSwd8UIcjSGbcYd6dge4myeaj-edK-oBOAtwen8hzSdMIMr5mxgysCKEOVL7kNkj-5IaGsGO_C2-r1vNy66wNdxto0wwNU2n6IBhCXsWfcWqolkJ7vLKZ3NBsvxVWyhLw2BCrTG6pz--Q=s2048)**


공격 대상 AP의 정보를 파악했다면 '`iwlist frequency` '명령어로 현재 wlan0 이 어떤 채널에 있는지 확인한다. 공격할 AP 와 wlan0의 채널이 일치하지 않다면 '`iwconfig wlan0 channel [채널번호]`' 명령어를 입력하여 공격할 AP 채널과 같게 설정한다.

>  kali# iwconfig wlan0 channel 11

**![](https://lh3.googleusercontent.com/Mn6XEecYzQ8ZgUi0gxF1iB4HdRGaThjEcmATenydkzP4Vme5xfPDOCaSnv88aHTaGOEZg2qbv3KmuWLIsu_YVOED7SEQsVeGncC2COkVI8UkVzgI6xw60x1PomkffNj4TdIcybjqyYayDx4MA7GOdxs2ikvwAMlKTuXNQSWHpWcTXY93CL0Hq9LluViLng)**

### 2) Packet Collection

'`airodump-ng -c [채널번호] —bssid [AP의 MAC주소] -w [저장할 파일 이름] wlan0`' 명령어를 입력해 Layer7 AP의 채널로 고정하고 IV를 수집할 파일을 생성한다.

   >kali# airodump-ng -c 11 —bssid 64:E5:99:63:5C:A4 -w hack_wep wlan0

 - `-c` 는 채널을 의미한다.
 - `—bssid` 는 AP의 MAC 주소를 의미한다.
 - `-w` 는 캡처 파일을 만든다.

**![](https://lh4.googleusercontent.com/AryWXVChJ6VjBjoAUXIDj9XcvB4UCAJNFkUH0rAo7pjWzzhfyG3Ald8M5uJBYGJ3Irt5OADtIHhzPlvI-17FjWC_wryp65UE4ld-j0D_LRThIWTDK_3tA9TNAGTvAnmgdfv-_vEW93fbPN3oyuCgXQ=s2048)**

이때 Layer7 AP를 사용하는 사람이 없거나 적다면 AP와 클라이언트 사이에 송수신되는 데이터양 또한 적어지기 때문에 패킷 수집이 원활하지 않을 수도 있다. 실제로 사진에서 보이듯이 AP에 연결된 단말기는 한 개뿐이기 때문에 패킷 수집이 어려울 것이다.

 이럴 땐 물리적으로 공격을 시도하여 패킷을 수집할 수 있다. 본 프로젝트에서는 ARP request replay 공격 (ARP 요청 공격) 과 Deauthentication 공격을 사용하여 의도적으로 많은 패킷을 발생시켜 수집할 것이다. 공격을 위해 선택한 단말기의 STATION(MAC 주소)를 기억해둔다.

### 3) ARP Request Replay Attack
이 공격은 AP ARP 패킷을 받을 때까지 기다리다가 받은 ARP 패킷을 캡처한 다음 트래픽에 주입시키는데, 이때 AP는 IV로 새 패킷을 생성해야 한다. 이 과정이 반복되면서 IV가 발생되고, 발생 패킷들을 수집할 수 있게 된다.

‘`aireplay-ng -3 -b [AP의 MAC주소] -h [단말기의 MAC주소] wlan0`’ 명령어를 입력하면 ARP 요청 패킷의 개수와 *ACK의 개수를 알려준다. 어떤 단말기가 AP에 접속하면 해당 AP에게 ARP 패킷을 보내게 될 때 ARP 요청 패킷의 개수가 늘어날 것이고, 패킷을 재전송할 때 전송이 성공적으로 진행됐다면 ACK의 개수가 늘어날 것이다. 

*ACK : acknowledge의 약자로 응답 문자라는 뜻이다. 데이터 전송이 성공적으로 이루어졌을 때 수신 측 컴퓨터가 송신 측 컴퓨터에 되돌려주는 코드 값이다.

   > kali# aireplay-ng -3 -b 64:E5:99:63:5C:A4 -h 9A:61:BA:EB:FF:9F wlan0

 - `-3` 은 aireplay-ng의 공격 옵션으로, ARP 요청 공격을 의미한다.
 - `-b` 는 aireplay-ng의 필터 옵션으로, AP의 MAC 주소 지정을 의미한다.
 - `-h` 는 aireplay-ng의 재생 옵션으로, 출발지 MAC 주소 지정을 의미한다.

![](https://velog.velcdn.com/images/yyyyyy/post/bf303ef6-417f-401c-a0ba-fbeeb7c31e9a/image.png)

우리의 목표는 IV를 많이 수집해 크랙을 시도하는 것이다. 따라서 IV을 수집하기 위해선 ARP 패킷을 받아야 한다. AP에 단말기가 연결될 때까지 기다릴 수도 있지만 빠른 진행을 위해 Deauthentication 공격을 사용한다.



### 4) Deauthentication Attack

이 공격은 AP를 사용하고 있는 단말기에게 Deauthentication 패킷을 전송해 AP와 단말기 간의 연결을 강제로 해제시킨다. Deauthentication 패킷이란 AP가 단말기에게 연결을 해제할 때 보내는 패킷이다.

‘`aireplay-ng -0 [공격할 횟수] -a [AP의 MAC주소] -c [단말기의 MAC주소] wlan0`’ 명령어를 입력하면 *DeAuth 패킷을 보냈다고 뜬다. 즉, 지정한 단말기에게 연결을 끊는 패킷을 보낸 것이고 실제로 단말기의 WiFi 설정을 보면 잠시 동안 와이파이가 끊겼다가 다시 연결되는 것을 볼 수 있다.

*DeAuth : Deauthentication 패킷의 줄임말

>kali# aireplay-ng -0 15 -a 64:E5:99:63:5C:A4 -c 9A:61:BA:EB:FF:9F wlan0

 - `-0` 은 aireplay-ng의 공격 옵션으로, 인증 해제 공격을 의미한다.
 - `-a` 는 aireplay-ng의 필터 옵션으로, AP의 MAC 주소 지정을 의미한다.
 - `-c` 는 aireplay-ng의 필터 옵션으로, 목적지 MAC 주소 지정을 의미한다


**![](https://lh5.googleusercontent.com/4Np_ZHAhC2avRAPbC6S6XPAW3oD2cai0_1KsXQUSs4qEZ6e3qduAOLxVQQAyQyQIc11B5KiZy_wb1PH-aLa0kK0QDoRUq2Q4l_-GaXYF17neHnX7gg0zD9O_QM19L2TOrjgConacjPz5ti98lu3WZg=s2048)**

단말기의 와이파이가 끊겼다가 다시 연결될 때, 목차 _3) ARP Request Replay Attack_ 에서 진행하고 있는 ARP 요청 공격 화면에서 읽는 패킷의 속도가 빨라지면서 ARP 요청 패킷의 개수와 ACK의 개수가 늘어나고 있는 것을 확인할 수 있다.
**![](https://lh5.googleusercontent.com/ZYY-rPkkNi3Mme5l3zSR1dvIbNzEy5FrI1NLh_sab6EmrRm44CdsUJir4VUYVNAbDJnYrrV46HCfigy1pwlQO8-PhJsbDP2MpAuTv7s1Op2AwXAoxMqlsJlO2om7ckWQUoEJPKOlkLF9yD5bNzKInw=s2048)**

또한 목차 _2) Packet Collection_ 의 IV를 수집하고 있는 화면에서 단말기의 Frames 과 AP의 #Data(브로드캐스트 패킷을 포함하여 캡처된 데이터 패킷 수)가 급격하게 늘어나고 있는 것을 볼 수 있다. #Data의 값은 IV의 값이므로 크랙을 성공시킬 수 있을 만큼 #Data가 충분히 쌓일 때까지 기다린다.

![](https://lh4.googleusercontent.com/yQtDatCvfjLHnEWQWP7p7sXU2MqCv2cHLshmd0g7VpdNSx8-tiMlWaHvZ1TRi8dF1Xa5k37I0OUEa3Wwa9-3__EOSjLA-bl3Fdq56mXLeRmlyZJUMb8mCSHEaXXj1wHRZUrcs8CRu6IcITc3S9J4Mg=s2048)이때 크랙을 성공시킬 만큼 충분한 IV의 값의 기준은 최소 5,000개이다. (목차 _1-1. WEP_ 중 _2) WEP이 취약한 이유_ 참고) 만약 IV를 약 1,000개의 양으로 크랙을 시도해 본다면 실패했다고 뜨며 5,000개 이상의 IV를 수집 후 시도하라는 문구를 볼 수 있다.
본 프로젝트에선 실패 없이 크랙에 성공하기 위해 약 50,000개의 IV를 모은 파일로 크랙에 시도했다.

### 5) Crack
'`aircrack-ng [저장한 파일 이름.cap]`' 명령어로 크랙을 시도한다.

 >kali# aircrack-ng hack_wep.cap


![](https://lh6.googleusercontent.com/xCPX9kHtX8Vv6Z4cUaSdwZnaW7fJep1ZP2KsNisKGYhVRAbfpr_jsW0GzZ8dsUoTO6nhpVHoco2rNLTCxPgsynUjP4sX_YwHW480qplbAPqbsueQ0npesKiDZ9V1xMiYShO5htAEzE_Vt5vq5jAuKQ=s2048)
‘KEY FOUND!’란 문구가 뜨며 Layer7 AP의 암호 키는 4C:61:79:65:72 = Layer 임을 알 수 있다.

## 3-3. 실습 - WPA/WPA2 key crack

처음에 '`iwconfig`' 명령어로 연결된 무선 랜 카드인 wlan0을 확인하면 Mode:Managed로 설정되어 있다. Managed 모드는 칼리 리눅스에 직접 연결된 패킷만 캡처한다.

범위 내에 있는 모든 패킷을 캡처하기 위해서 '`airmon-ng start wlan0`' 명령어를 입력해 Mode:Monitor (무차별 모드)로 전환한다.

>kali# airmon-ng start wlan0
    

![](https://velog.velcdn.com/images/yyyyyy/post/224f6a7e-b5d1-4341-a886-6431adccd65f/image.png)

### 1) Reconnaissance

‘`airodump-ng wlan0`’ 명령어를 입력해 주변의 AP를 검색한다. 본 실습의 목표 대상은 WPA2를 사용하는 실습용 Layer7 AP이다. 공격 대상이 될 AP를 정했다면 해당 AP의 BSSID(MAC 주소)와 CH(채널 번호)를 기억해둔다.

  >kali# airodump-ng wlan0


![](https://lh3.googleusercontent.com/uZTXNQNJh7xsuQP0L_Cn4tpNrzDA77o9voCxI5kwnkHj9vfFc2gQz8yL0RxA1R3-DgX3Cwbc9Rs5MkvHAsQxPRMqRiIQQHlfsHlW_waPPrzwKGPSipKDrhvgkduTjJigvb5v6GOReZYyF13hbE89kw=s2048)공격 대상 AP의 정보를 파악했다면 ‘`iwlist frequency`’ 명령어로 현재 wlan0이 어떤 채널에 있는 지 확인한다. 공격할 AP와 wlan0의 채널이 일치하지 않다면 ‘`iwconfig wlan0 channel [채널번호]`’ 명령어를 입력하여 공격할 AP 채널과 같게 설정한다. 

 >kali# iwconfig wlan0 channel 11


**![](https://lh6.googleusercontent.com/RnWTl1799l4l-yDAZDANqPwHRmjKgC7azpE4F_dheSwTQ8NXRk87Q_allhX0tYXRA4sb7mwhv4s4i5gnvXrbWr31NZdlPH-FeFDgwlTndrGV1Y7b2gsst8YrSUcTRxIuE8l--OAnwkn5pUzb9S_xbQGG5Fk0Ke0fDBvUsSxXRUBxXDKmZ-QT6b4bazV0rA)**


### 2) Packet Collection

‘`airodump-ng -c [채널번호] —bssid [AP의 MAC주소] -w [저장할 파일 이름] wlan0`’ 명령어를 입력해 Layer7 AP의 채널로 고정하여 다른 채널의 간섭을 줄인다. 또한 4-Way Handshaking 과정에서 주고 받는 데이터를 수집할 파일을 생성한다.

 >kali# airodump-ng -c 11 --bssid 64:E5:99:63:5C:A4 -w hack_wpa2 wlan0

 - `-c` 는 채널을 의미한다.
 - `—bssid` 는 AP의 MAC 주소를 의미한다.
 - `-w` 는 캡처 파일을 만든다.

![](https://lh5.googleusercontent.com/Kc2C4qaiY09RScmJ3WbulejQuSumnn36xG_ztin4NV9Dg-pmeuILImoxEo-FTJn72jpgf4ZVP4kN6A7eG_n7t1p4bWp8mTcDBSzRt85YifZY4Oyg_NEE8-65XE8_18Oqbb2Hc1Ee7LH8u4J9ScH_1g=s2048)WPA/WPA2의 키를 크랙하기 위해선 4-Way Handshaking 과정에서 주고받는 데이터 패킷이 필요하다. 4-Way Handshaking 은 단말기가 AP와의 연결 해제를 시도할 때 거치는 인증 과정으로 단말기가 AP에 연결 해제를 시도할 때까지 기다리는 방법도 있지만 빠른 진행을 위해 Deauthentication 공격을 사용한다.

공격을 위해 사용할 단말기의 STATION(MAC 주소)를 기억해둔다.

### 3) Deauthentication Attack
이 공격은 AP를 사용하고 있는 단말기에게 Deauthentication 패킷을 전송해 AP와 단말기 간의 연결을 강제로 해제시킨다. Deauthentication 패킷이란 AP가 단말기에게 연결을 해제할 때 보내는 패킷이다.
‘`aireplay-ng -0 [공격할 횟수] -a [AP의 MAC주소] -c [단말기의 MAC주소] wlan0`’ 명령어를 입력하면 *DeAuth 패킷을 보냈다고 뜬다. 즉, 지정한 단말기에게 연결을 끊는 패킷을 보낸 것이고 실제로 단말기의 WiFi 설정을 보면 잠시 동안 와이파이가 끊겼다가 다시 연결되는 것을 볼 수 있다.

*DeAuth : Deauthentication 패킷의 줄임말

 >kali# aireplay-ng -0 10 -a 64:E5:99:63:5C:A4 -c 9A:61:BA:EB:FF:9F wlan0

 - `-0` 은 aireplay-ng의 공격 옵션으로, 인증 해제 공격을 의미한다.
 - `-a` 는 aireplay-ng의 필터 옵션으로, AP의 MAC 주소 지정을 의미한다.
 - `-c` 는 aireplay-ng의 필터 옵션으로, 목적지 MAC 주소 지정을 의미한다.
![](https://lh3.googleusercontent.com/dGNnp-mRRO5Hlg6JTrCR4W23YTyN8C67XfNxtLHGSkN7FsuT1AdRjrG-DrhL6orp4MrxArL73fjnKbhvneVbC1E_ZMjnZkOw5Gmy8BUgdfL7yuW95LzOqPAHf1u4iS8OgK93liZsbI2raFqafmMpyQ=s2048)

AP와 단말기와의 연결이 해제되는 과정에서 4-Way Handshaking 데이터 패킷을 수집했고, 그 증거로 목차 2) Packet Collection 에서 4-Way Handshaking 데이터 패킷을 수집하고 있는 화면의 오른쪽 위에 WPA handsake: 헤더가 생긴 것을 확인할 수 있다.

![](https://lh5.googleusercontent.com/ETmTb4mUDrmQ7VxtOyxq4er5dtZUH6PSSohoU4m_Q68ncLglyNHXJz8PHhuwYfNI0nian1ureJgS6jQPunPXqAyy5KlcLQC395QDwSVsO5K0Loghm8FDnTnHNXvZODXoljZQhT1PAER-VGhRaizgEA=s2048)

### 4) Crack - Dictionary attack

Dictionary 공격에 필요한 사전 파일을 만들기 위해 칼리 리눅스가 제공하고 있는 crunch 도구를 사용한다. ‘`crunch [최소 길이] [최대 길이] [조합할 숫자/문자] -o [저장할 파일 이름]`’ 명령어를 입력해 사전 파일을 만든다.

  >kali# crunch 8 8 1234567890 -o password.lst

 - `-o` 은 output의 약자로 파일을 생성하겠다는 의미이다.
 
![](https://lh6.googleusercontent.com/djlfzXmxW2Y7KhQYQZvsDRUidNZ8PGA8NBRkws5TpioSEv2fC0EhVDVz1IxXVITRJkZp9NQFJwax1odaHxCUFKt4e-svYINQlLGlIjufwpl9IOcsn0j6zeXRZi9TVGl1r7Vq5Kg8p17RDnsVEIcvSw=s2048)
crunch를 사용하여 최소 8 자리에서 최대 8 자리까지 숫자1234567890 을 조합하여 password.lst에 저장한 파일이 생성되었다. 생성된 파일로 사전 대입 공격을 진행한다. ‘`aircrack-ng -w [만든 .lst 파일 이름] -b [AP의 MAC주소] [4-Way Handshaking 데이터 패킷 저장 파일 이름.cap]`’ 명령어를 입력한다.

  >kali# aircrack-ng -w password.lst -b 64:E5:99:63:5C:A4 hack_wpa2.cap

 - `-w` 은 사전 파일 경로 지정을 의미한다.
 - `-b` 는 AP의 MAC 주소를 의미한다.

![](https://lh5.googleusercontent.com/sYAury6nWm8WNxMA83UAhNL48tEPC4qvEDellDXldpBqkeaawNKLl-U_zdzkcz8NT9FJWeEvxVDHsWVj-m3gxBaG8MNsbSuChIvVGTRaXSRA0FYqeMcXnAj0VrRKhTPlkBuAGoeCdttNgukP43yo8Q=s2048)
KEY FOUND! 란 문구가 뜨며 Layer7 AP의 암호 키는 15974270 임을 알 수 있다.

---


# 4. 무선 와이파이 취약점에 따른 보안 방안

## 4-1. 개인적 차원

### 1) 공유기의 펌웨어 업데이트
정기적으로 공유기의 펌웨어를 업데이트하여 최신 버전의 보안을 유지한다. 또한 현재 가장 보안 강도가 높은 암호화 방식인 WPA2로 설정한다.

### 2) 공유기 비밀번호 관리
공유기를 처음 설정할 때 사용하는 초기 비밀번호 대신 보안 수준이 높은 비밀번호로 변경한다. 또한 일정 시간 동일한 비밀번호를 사용할수록 보안성이 낮아지므로 주기적으로 변경한다.

### 3) VPN 사용 및 방화벽 활성화
불가피하게 공공 와이파이를 사용해야 할 때 방화벽을 활성화시키고 VPN을 사용한다. VPN을 사용하면 해커가 사용자의 트래픽을 모니터링하고 있어도 사용자의 트래픽은 VPN 서버를 거치기 때문에 사용자의 PC가 VPN과 정보를 송수신 하고 있는 것만 볼 수 있다.

### 3) 공공 와이파이 사용에 대한 경각심 고취
공공 와이파이 사용자들은 신뢰할 수 있는 인증된 와이파이만을 사용하며 공공 와이파이를 사용하고 있는 동안엔 온라인 뱅킹 등 각종 중요한 개인정보를 사용하는 업무는 해킹의 위험이 크므로 자제한다. 또한 개인정보를 취급하는 앱들은 사용자에게 ‘와이파이로 이용할 시 해킹될 위험이 있습니다.’ 와 같은 알림창을 띄워 경각심을 일깨워 준다.

## 4-2. 국가적 차원

### 1) 해킹 탐지서비스 제공
서비스를 제공받고자 하는 국민들에게 개인정보 동의를 구하여 동의한 국민들의 최소한의 정보만을 수집하고 국민들이 접속하고자 하는 와이파이가 해킹되었는지 탐지하여 알리고, 해킹 위험이 있을 시 해당 와이파이의 접속을 차단시킨다.

### 2) 무선 랜 보안에 대한 대국민 인식 제고 
현재 무선랜에 대한 보안 기술이 부족하다기보다는 사용자들의 보안인식 부족으로 인한 해킹 피해 사례가 늘어나고 있으므로 정부에선 공유기 보안 설정에 대한 수칙들 지정하고 버스 및 지하철 등 국민들이 많이 이용하는 곳에 배포 및 홍보를 하여 국민들이 무선 랜 보안에 대한 정보를 쉽게 접할 수 있게 한다.

---
## 참고 문헌

[1] 방주원 외, 『보안 실무자를 위한 네트워크 공격 패킷 분석』, 프리렉, 2019, p.510-552.

[2\] 배희라, 김민영, 송수경, 이슬기, 장영현, 『무선공유기 보안공격 분석 및 무료와이파이해킹 해결방안』, 2016.]

[3\] 백종현, 박순태, 『국내 무선랜(WiFi) 보안 운영 현황 및 정책 방향』, 2011.]


---
https://drive.google.com/drive/folders/17nkk7fMoZ1Sw7wTHExfIt6y6jusp883j?usp=sharing
# 📡

