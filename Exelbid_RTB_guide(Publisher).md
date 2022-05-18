Publisher RTB 연동 가이드
=======================

  * [1. ExelBid 소개](#1-exelbid-소개)
    * [1.1 ExelBid RTB?](#11-exelbid-rtb)
    * [1.2 ExelBid 연동 절차](#12-exelbid-연동-절차)
    * [1.3 ExelBid History](#13-exelbid-history)
  * [2. OpenRTB Basics](#2-openrtb-basics)
    * [2.1 전송](#21-전송)
    * [2.2 Data Format](#22-data-format)
    * [2.3 OpenRTB Version HTTP Header](#23-openrtb-version-http-header)
    * [2.4 ExelBid 연동 사항](#24-exelbid-연동-사항)
      * [2.4.1 지원 배너 종류](#241-지원-배너-종류)
  * [3. 입찰 요청(Bid Request Specification)](#3-입찰-요청bid-request-specification)
    * [3.1 Object Model (요청 오브젝트 모델 설명)](#31-object-model-요청-오브젝트-모델-설명)
    * [3.2 Object Specifications](#32-object-specifications)
      * [3.2.1 Object: BidRequest](#321-object-bidrequest)
      * [3.2.2 Object: Imp](#322-object-imp)
        * [3.2.2.1 Object: Ext](#3221-object-ext)
      * [3.2.3 Object: Banner](#323-object-banner)
      * [3.2.4 Object: Video](#324-object-video)
      * [3.2.5 Object: Native](#325-object-native)
      * [3.2.6 Object: Site](#326-object-site)
      * [3.2.7 Object: App](#327-object-app)
      * [3.2.8 Object: Publisher](#328-object-publisher)
      * [3.2.9 Object: Content](#329-object-content)
      * [3.2.10 Object: Device](#3210-object-device)
      * [3.2.11 Object: Geo](#3211-object-geo)
      * [3.2.12 Object: User](#3212-object-user)
      * [3.2.13 Object: Data](#3213-object-data)
      * [3.2.14 Object: Segment](#3214-object-segment)
      * [3.2.15 Object: Pmp](#3215-object-pmp)
      * [3.2.16 Object: Deal](#3216-object-deal)
  * [4. 입찰 응답(Bid Response Specification)](#4-입찰-응답bid-response-specification)
    * [4.1 Object Model](#41-object-model)
    * [4.2 Object Specifications](#42-object-specifications)
      * [4.2.1 Object: BidResponse](#421-object-bidresponse)
      * [4.2.2 Object: SeatBid](#422-object-seatbid)
      * [4.2.3 Object: Bid](#423-object-bid)
      * [4.2.4 Object: Ext](#424-object-ext)
    * [4.3 치환(Substitution Macros)](#43-치환substitution-macros)
  * [5. Native 규격](#5-native-규격)
    * [5.1 입찰 요청](#51-입찰-요청)
    * [5.2 입찰 응답](#52-입찰-응답)
  * [6. 참조 목록표(Enumerated Lists Specification)](#6-참조-목록표enumerated-lists-specification)
  * [7. 비딩 요청/응답 예제(Bid Request/Response Samples)](#7-비딩-요청응답-예제bid-requestresponse-samples)
    * [7.1 Bid Requests](#71-bid-requests)
      * [7.1.1 Example 1 (이미지 광고 요청)](#711-example-1-(이미지-광고-요청))
      * [7.1.2 Example 2 (Native 광고 요청)](#712-example-2-(native-광고-요청))
    * [7.2 Bid Responses](#72-bid-responses)
      * [7.2.1 Example 1 (이미지 광고 응답)](#721-example-1-(이미지-광고-응답))
      * [7.2.2 Example 2 (Native 광고 응답)](#722-example-2-(native-광고-응답))


### 1. ExelBid 소개

#### 1.1 ExelBid RTB?
***OpenRTB Specification version 2.3, OpenRTB-Native-Ads-Specification 1.0*** 을 기반으로 합니다. 다만, 해당 Object를 완전히 지원하지 않으며, 매체와 연동시 상호 검토가 전제 되어 유연하게 적용 될 수 있습니다.

#### 1.2 ExelBid 연동 절차
**연동 전 사전준비**
1) 엑셀비드 담당자가 SSP 파트너용 Questionnaire를 전달합니다.
2) SSP 파트너가 Questionnaire를 작성하여 회신하면 엑셀비드 담당자가 규격 검토 후, 연동 가능 여부를 안내합니다.
3) SSP 파트너는 엑셀비드 대시보드 사이트(manage.exelbid.com)에 가입 후, 가입 정보를 엑셀비드 담당자에게 전달합니다.
4) 엑셀비드 담당자가 해당 계정에 대해 대시보드 접근 권한을 부여합니다.
5) 엑셀비드에서 endpoint URL을 발급하면 SSP 파트너는 해당 URL의 정합성을 확인합니다.

**연동 테스트 및 실거래**
1) 본격적인 연동에 앞서 양사 간 연동 테스트를 진행합니다.
2) 테스트는 일반적으로 영업일 기준 약 5일에 걸쳐 진행되며, 상황에 따라 유동적으로 변동 가능합니다.
3) 테스트를 통해 양사 규격의 호환성, 필수값 포함 여부, 집계 수치 오차율 등을 확인합니다.
4) 연동 테스트 시, 최초의 광고요청에 대해서는 엑셀비드의 광고응답이 나가지 않습니다(no-bid). 이는 엑셀비드 시스템 상에 트래픽 정보가 등록되는 시간이 필요하기 때문입니다. 등록 과정에는 평균 5~10분이 소요됩니다.
5) 테스트를 통해 이상이 없다는 것이 확인되면 테스트를 종료합니다. 양사간 거래 계약서를 체결한 후, 실거래를 시작합니다.

**정산**

&nbsp;&nbsp;&nbsp;매월 초, 전월자 거래 금액에 대해 세금계산서를 발행합니다. 거래 금액을 확인하는 방법은 다음과 같습니다.

1) 엑셀비드 대시보드 사이트(manage.exelbid.com)에 접속합니다.
2) Invoice 메뉴를 클릭합니다.
![SSP파트너 정산과정 1](image/ssp_partner_billing_1.jpg)

3) 해당 정산 월과 정산 총금액(Revenue), 세금액(VAT)을 확인합니다.
![SSP파트너 정산과정 2](image/ssp_partner_billing_2.jpg)

4) 내용 확인 후, 이상 없을 시 해당 금액으로 세금계산서를 발행합니다.

*위 내용과 관련하여 자세한 사항은 엑셀비드 담당자에게 문의 부탁드립니다.*


### 2. OpenRTB Basics

#### 2.1 전송

기본 프로토콜은 HTTP(S)입니다. HTTP POST가 입찰 요청에 사용됩니다. No Bid(광고 없음) 경우 HTTP코드 204리턴됩니다.

#### 2.2 Data Format

JSON을 입찰 요청과 응답의 전문으로 사용합니다. 입찰 요청 *Content-type* 으로 *Content-Type: application/json* 이 지정되며, 입찰 응답 포맷도 같이 적용 됩니다..

```
Content-Type: application/json
```

#### 2.3 OpenRTB Version HTTP Header

OpenRTB 버전을 입찰 요청의 헤더에 포합니다. 

```
x-openrtb-version: 2.3
```

#### 2.4 ExelBid 연동 사항

##### 2.4.1 지원하는 광고 종류

  - 디스플레이 광고(띠배너, 전면배너, 네이티브)
  - 비디오 광고

### 3. 입찰 요청(Bid Request Specification)

RTB 시작은 입찰 요청을 보내면서 시작됩니다. BidRequest는 하나 이상의 Imp(impression) Object로 구성됩니다.

#### 3.1 Object Model (요청 오브젝트 모델 설명)

#### 3.2 Object Specifications

##### 3.2.1 Object: BidRequest

입찰 최상위 오브젝트. 입찰 고유 값인 id, imp, app or site 의 필수 오브젝트가 포함되어야 합니다. 필요에 따라 device(기기정보) 오브젝트, 통화 종류를 나타내는 cur 값들, 또 테스트 입찰값인 test 등이 권장 사항으로 포함됩니다.

 Name   | Type         | 필수, 기본값  | Description                                                               
:-------|:-------------|:--------------|:--------------------------------------------------------------------------
 id     | string       | 필수          | 입찰에 대한 유니크 아이디                                                 
 imp    | object array | 필수          | imp 오브젝트 배열                                                         
 site   | object       | 필수(or app)  | app 개체와 site 오브젝트 둘 중 하나가 반드시 포함 된다.                   
 app    | object       | 필수(or site) | app 개체와 site 오브젝트 둘 중 하나가 반드시 포함 됩니다.                 
 device | object       |               | 광고가 전송될 디바이스 특성, 종류등을 설명합니다.                         
 user   | object       |               | 사용자 정보를 나타냅니다.                                                 
 test   | integer      | 기본값 0      | 0 or 1로 기본값은 0, 1일 경우 테스트 연동                                 
 at     | integer      | 기본값 2      | 낙찰 타입. 입찰 금액들 중 낙찰 금액 결정 순위( 기본값 2)                  
 tmax   | integer      |               | 입찰에 참여하기 위한 밀리세턴드 단위 최대 지연시간                        
 cur    | string array | 기본값 "USD"  | ISO–4217 코드의 단위통화 리스트.                                          
 bcat   | string array |               | 제외되어야 할 광고주 카테고리 리스트<br/>IAB OpenRTB Spec 2.3 > 표 5.1 조
 badv   | string array |               | 제외되어야 할 광고주의 최상위 도메인 리스트.                              

##### 3.2.2 Object: Imp

입찰 대상이 되는 광고의 위치나 광고 종류 등을 나타냅니다.

 Name              | Type    | 필수, 기본값    | Description                                                  
:------------------|:--------|:----------------|:-------------------------------------------------------------
 id                | string  | 필수            | BidRequest 오브젝트 안에서 imp를 구분하기 위한 고유 식별자   
 banner            | object  | *필수           | banner, video, native 오브젝트중 하나 이상을 포함하고 있어야 합니다.
 video             | object  | *필수           | banner, video, native 오브젝트중 하나 이상을 포함하고 있어야 합니다.
 native            | object  | *필수           | banner, video, native 오브젝트중 하나 이상을 포함하고 있어야 합니다.
 pmp               | object  |                 | Pmp 오브젝트는 이 Imp 오브젝트에 대한 PMP 협약을 포함합니다.
 displaymanager    | string  |                 | 노출 sdk 이름                                                
 displaymanagerver | string  |                 | 노출 sdk 버전                                                
 instl             | integer | 기본값 0        | 전면광고 여부. 1일 경우 전면                                 
 tagid             | string  |                 | 노출 인벤토리(해당 지면, 유닛)의 고유한 식별자               
 bidfloor          | integer | 기본값 0        | Impression의 입찰 최저가                                     
 bidfloorcur       | string  | 기본값 "USD"    | ISO–4217 알파벳 코드를 사용하여 명시되어야 합니다            
 ext               | object  |                 | click_callback_url 포함. 3.2.2.1 Object: Ext 참조   
 

##### 3.2.2.1 Object: Ext

  Name              | Type    | 필수, 기본값    | Description                                                  
 :------------------|:--------|:----------------|:-------------------------------------------------------------
  click_callback_url | string  |                 | Exelbid가 click 트리킹시 해당 click_callback_url을 호출
  rewarded          | integer |  기본값 0       | 1 : 리워드

##### 3.2.3 Object: Banner

디스플레이 광고. native, video가 아닌 일반 광고일 경우 반드시 포함되어야 합니다.

 Name     | Type          | 필수, 기본값 | Description                                                                          
:---------|:--------------|:-------------|:-------------------------------------------------------------------------------------
 id       | string        |              | 비디오 등의 경우 여러 개의 banner 오브젝트 존재 시 고유한 식별자. 현 버전 의미 없음.
 w        | integer       | 필수         | 광고의 넓이(pixel)                                                                   
 h        | integer       | 필수         | 광고의 높이(pixel)                                                                   
 btype    | integer array |              | 제외 되어야 할 광고물 종류<br>IAB OpenRTB Spec 2.3 > 표 5.2 참조                     
 battr    | integer array |              | 제외 되어야 할 광고물 속성<br>IAB OpenRTB Spec 2.3 > 표 5.3 참조                     
 pos      | integer       | 기본값 0     | 광고 위치. IAB OpenRTB Spec 2.3 > 표 5.4 참조                                        
 topframe | integer       | 기본값 0     | 배너가 최상위 프레임에 전송되는지 여부.<br>1 최상위, 0 최상위 아님    

##### 3.2.4 Object: Video
비디오 광고 

 Name     | Type          | 필수, 기본값 | Description                                                                          
:---------|:--------------|:-------------|:-------------------------------------------------------------------------------------
 mimes    | string array  | 필수         | 지원하는 컨텐츠 MIME 타입들
 minduration | integer  | 필수           | 비디오광고 최소 길이 초.
 maxduration | integer  | 필수           | 비디오광고 최대 길이 초.
 protocols | integer array  | 필수       | 지원하는 비디오광고 프로토콜 리스트 <br>IAB OpenRTB Spec 2.3 > 표 5.8 참조  
 w        | integer       | 필수         | 비디오 플레이어의 넓이 픽셀.  
 h        | integer       | 필수         | 비디오 플레이어의 높이 픽셀.
 startdelay | integer     |             | 광고가 시작되는 시작 딜레이 초단위, pre-roll, mid-roll, post-roll 광고 위치.<br>IAB OpenRTB Spec 2.3 > 표 5.10 참조
 linearity | integer      |             | 광고의 linearity 여부 <br>IAB OpenRTB Spec 2.3 > 표 5.7 참조                     
 sequence  | integer      |            | 여러개의 impression일 경우 시퀀스 숫자.
 battr     | integer      |             | 블락 크리에티브 속성  <br>IAB OpenRTB Spec 2.3 > 표 5.3 참조  
 boxingallowed | integer      | 기본값 1  | 크리에티브 사이즈가 달라도 박싱처리해서 플레이 하는지 여부. 0 = no, 1 = yes
 playbackmethod | integer array |       | IAB OpenRTB Spec 2.3 > 표 5.9 참조  
 companionad | object array |       | Banner 오브젝트 리스트. 컨페니언광고가 있을경우 사용.
 companiontype | integer array |       | 지원하는 VAST 컨패니언 광고 종류.
 api | integer array |       | 지원하는 API framework 리스트 <br>IAB OpenRTB Spec 2.3 > 표 5.6 참조                 

##### 3.2.5 Object: Native

Native 형식, 광고가 기존 컨텐츠(ex. 트위터, 페이스북등)에 유사하게 혼합되기 위함으로, Open RTB Native Spec에 의해 응답 됩니다.

 Name    | Type          | 필수, 기본값 | Description                                                                                        
:--------|:--------------|:-------------|:---------------------------------------------------------------------------------------------------
 request | string        | 필수         | Native Ad Spec 을 준수하는 요청 페이로드<br>Open RTB Native Spec 에 의거한 Json stirng이 반영된다.
 ver     | string        |              | Native Ad Spec 버전                                                                                
 battr   | integer array |              | 제외 되어야 할 광고물 속성 <br>IAB OpenRTB Spec 2.3 > 표 5.3 참조                                  

##### 3.2.6 Object: Site

광고가 전송될 지면이 웹사이트일 경우 반드시 포함되어야 합니다. site오브젝트와 app오브젝트와 동시에 포함 할 수 없습니다.

 Name       | Type         | 필수, 기본값 | Description                                                    
:-----------|:-------------|:-------------|:---------------------------------------------------------------
 id         | string       | 필수         | ExelBid와 연결된 사이트 ID.                                    
 name       | string       | 필수         | 사이트 이름                                                    
 domain     | string       | 필수         | 사이트 도메인. 광고주에서 블럭 처리하는데 사용 할 수 있습니다.
 cat        | string array |              | 전체 IAB 카테고리 리스트                                       
 sectioncat | string array |              | 현재 섹션의 IAB 카테고리 리스트                                
 mobile     | integer      |              | 모바일 최적화 여부 0 = no, 1 = yes                             
 publisher  | object       |              | publisher 상세 정보        
 content    | object       |             | content 상세 정보                                        

##### 3.2.7 Object: App

광고가 전송될 지면이 어플리케이션일 경우 반드시 포함되어야 합니다. site오브젝트와 app오브젝트와 동시에 포함 할 수 없습니다.

 Name       | Type         | 필수, 기본값 | Description                                               
:-----------|:-------------|:-------------|:----------------------------------------------------------
 id         | string       | 필수         | ExelBid와 연결된 앱ID.                                    
 name       | string       | 필수         | 앱 이름                                                   
 bundle     | string       | 필수         | Android 패키지명, IOS에서는 패키지명 혹은 어플리케이션 ID
 domain     | string       |              | 어플리케이션 도메인.                                      
 storeurl   | string       |              | 앱스토어 URL                                              
 cat        | string array |              | 전체 IAB 카테고리 리스트                                  
 sectioncat | string array |              | 현재 섹션의 IAB 카테고리 리스트                           
 ver        | integer      |              | 어플리케이션 버전                                         
 paid       | integer      |              | 0 = 무료, 1 = 유료                                        
 publisher  | object       | 필수         | publisher 상세 정보 
 content    | object       |             | content 상세 정보                                   

##### 3.2.8 Object: Publisher

 Name   | Type         | 필수, 기본값 | Description                     
:-------|:-------------|:-------------|:--------------------------------
 id     | string       | 필수         | ExelBid와 연결된 publisher ID.  
 name   | string       | 필수         | publisher name                  
 cat    | string array |              | publisher의 IAB 카테고리 리스트
 domain | string       |              | 최상위 도메인                   

##### 3.2.9 Object: Content

(*) 항목은 IAB OpenRTB Spec 2.4 에서 가져온다.

 Name   | Type         | 필수, 기본값 | Description                     
:-------|:-------------|:-------------|:--------------------------------
 id     | string       |          | 컨텐츠 아이디 
 episode   | integer   |          | 에피소드 넘버(통상 비디오 컨텐츠)       
 title    | string     |          | 컨텐츠 제목
 series | string       |          | 컨텐츠 시리즈
 season | string       |          | 컨텐츠 시즌
 language | string     |          | 컨텐츠 지원 언어 ISO-639-1-alpha-2.
 genre    | string     | *         | 컨텐츠 장르
 data     | object array  | *         | 컨텐츠 데이타

##### 3.2.10 Object: Device

하드웨어, 플랫폼, 위치, 통신사등 해당 기기와 관련되 정보를 제공합니다.

 Name           | Type    | 필수, 기본값 | Description                                                     
:---------------|:--------|:-------------|:----------------------------------------------------------------
 ua             | string  |              | User Agent                                                      
 geo            | object  | 필수         | 위치 정보                                                       
 dnt            | integer |              | 브라우에 do-not-track 이 false로 세팅이면 0, true이면 1로 전달  
 ip             | string  |              | IPv4 주소                                                       
 devicetype     | integer | 필수         | 디바이스 종류. IAB OpenRTB Spec 2.3 > 표 5.17 참조              
 make           | string  |              | 디바이스 제조사                                                 
 model          | string  |              | 디바이스 모델                                                   
 os             | string  | 필수         | 디바이스 운영체제 (android, ios)                                
 osv            | string  |              | 디바이스 운영체제 버전                                          
 h              | integer |              | 디바이스 넓이(pixel)                                            
 w              | integer |              | 디바이스 높이(pixel)                                            
 language       | string  |              | 브라우저 언어. ISO-639-1-alpha-2                                
 carrier        | string  |              | 통신사 또는 IP 어드레스로 부터 유도된 ISP(인터넷 서비스 제공자)
 connectiontype | string  |              | 네트워크 연결 종류 IAB OpenRTB Spec 2.3 > 표 5.18 참조          
 ifa            | string  | 권장         | 광고 트래킹 아이디(ex) android = gaid, ios = idfa)                  

##### 3.2.11 Object: Geo

Device 오브젝트와 User 오브젝트 두 군데, 혹은 한 군데 모두 적용 될 수 있습니다.

 Name    | Type    | 필수, 기본값 | Description                                      
:--------|:--------|:-------------|:-------------------------------------------------
 lat     | float   |              | 위도                                             
 lon     | float   |              | 경도                                             
 type    | integer |              | 데이터 출처. IAB OpenRTB Spec 2.3 > 표 5.16 참조
 country | string  |              | 국가 코드. ISO-3166-1-alpha-3.                   

##### 3.2.12 Object: User

디바이스 사용자의 정보를 나타냅니다.

 Name     | Type    | 필수, 기본값 | Description                                                                        
:---------|:--------|:-------------|:-----------------------------------------------------------------------------------
 id       | string  |              | ExelBid에서 사용자를 구분하는 고유한 아이디                                        
 yob      | integer |              | 4자리 출생년도                                                                     
 gender   | string  |              | 성별 남자 = "M", 여자는 "F", 그외는 "O"                                            
 keywords | string  |              | key:value 형식을 콤마로 구분한 사용자 관련 키워드 리스트(ex = e_age:40,e_gender:F)
 geo      | object  |              | 위치 정보                                                                          

##### 3.2.13 Object: Data

다양한 출처(ex : exchange 자체, 제 3의 제공자 등)로 제공되는 추가적인 사용자 데이터를 그룹화 하여 하나의 Data로 제공합니다.

 Name    | Type         | 필수, 기본값 | Description                                 
:--------|:-------------|:-------------|:--------------------------------------------
 id      | string       |              | ExelBid에서 사용자를 구분하는 고유한 아이디
 name    | string       |              | 데이터 제공자                               
 segment | object array |              | 데이터를 포함하는 segment 오브젝트 배열     

##### 3.2.14 Object: Segment

상위 Data오브젝트에서 지정된 제공자로 부터의 일정 정보를 포함합니다.

 Name  | Type   | 필수, 기본값 | Description                       
:------|:-------|:-------------|:----------------------------------
 id    | string |              | 데이터를 구분하는 유니크한 아이디
 name  | string |              | 데이터 이름                       
 value | string |              | 데이터      

##### 3.2.15 Object: Pmp

 Imp오브젝트에 포함되며, Private MarketPlace 또는 직거래에서 RTB 프로토콜을 사용하기 위해 필요한
 정보를 포함합니다.

  Name              | Type   | 필수, 기본값 | Description                       
 :------------------|:-------|:------------|:----------------------------------
  private_auction   | string | 기본값 0     | 데이터를 구분하는 유니크한 아이디
  deals             | object |             | Imp에 해당하는 직거래 리스트를 내포하고 있는 deal 오브젝트 배열    

##### 3.2.16 Object: Deal

  구매자와 판매자간에 사전 협약된 거래로서, 상위 Imp가 이 계약 조건하에 가능하다는 것을 명시합니다.

   Name              | Type    | 필수, 기본값    | Description                                                  
  :------------------|:--------|:----------------|:-------------------------------------------------------------
   id                | string  | 필수            | BidRequest 오브젝트 안에서 imp를 구분하기 위한 고유 식별자   
   bidfloor          | integer | 기본값 0        | Impression의 입찰 최저가                                     
   bidfloorcur       | string  | 기본값 "USD"    | ISO–4217 알파벳 코드를 사용하여 명시되어야 합니다         
   wseat             | string array |            | 경매에 참여할 수 있는 입찰자격 코드 리스트.
   wadomain          | string array |            | 입찰할 수 있는 광고주 도메인 리스트.
   at                | integer |                 | 낙찰 타입. 입찰 금액들 중 낙찰 금액 결정 순위                     

### 4. 입찰 응답(Bid Response Specification)

#### 4.1 Object Model

기본적으로 하나의 입찰 요청에 대해 하나의 입찰 응답이 됩니다.

 Name        | Type   | 필수, 기본값 | Description                                                                                                                                           
:------------|:-------|:-------------|:------------------------------------------------------------------------------------------------------------------------------------------------------
 BidResponse | object | 필수         | 최상위                                                                                                                                                
 seatbid     | object | 필수         | 상위 BidResponse에 하나 이상의 seatbid가 존재한다.                                                                                               
 bid         | object | 필수         | 상위 seatbid에는 하나 이상의 bid가 존재한다.                                                                                                     
 ext         | object |              | 규격 표준을 벗어나는 OpenRTB 주체가 동의한 경우 이 오브젝트로 규격의 유연성을 제공합니다.

#### 4.2 Object Specifications

##### 4.2.1 Object: BidResponse

 Name    | Type         | 필수, 기본값 | Description                                                                              
:--------|:-------------|:-------------|:-----------------------------------------------------------------------------------------
 id      | string       | 필수         | Bid Request의 ID                                                                         
 seatbid | object array | 필수         |                                                                                          
 bidid   | string       |              | 응답의 ID로 입찰자에 의해 선택됩니다.              
 cur     | string       | 필수         | ISO–4217 코드의 단위통화.                                                                
 nbr     | integer      |              | IAB OpenRTB Spec 2.3 > 표 5.19 참조                                                      
 ext     |              |              | 규격 표준을 벗어나는 OpenRTB 주체가 동의한 경우 이 오브젝트로 규격의 유연성을 제공합니다

최상위 오브젝트로 id는 BidRequest의 ID를 그대로 사용합니다. 최소 하나의 seatbid 오브젝트가 필수이며, 하나의 imp에 대한 입찰을 포함합니다.

##### 4.2.2 Object: SeatBid

 Name | Type         | 필수, 기본값 | Description                                                                                                                                   
:-----|:-------------|:-------------|:----------------------------------------------------------------------------------------------------------------------------------------------
 bid  | object array | 필수         | imp 대한 응답 오브젝트                                                                                                                        
 seat | string       |              | 입찰하는 입찰 자격 코드                                                                                                                       
 ext  |              |              | 규격 표준을 벗어나는 OpenRTB 주체가 동의한 경우 이 오브젝트로 규격의 유연성을 제공합니다 동의한 경우 이 오브젝트로 규격의 유연성을 제공합니다

##### 4.2.3 Object: Bid

 Name    | Type         | 필수, 기본값 | Description                                                                                                                                                                                  
:--------|:-------------|:-------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 id      | string       | 필수         | 입찰자가 트래킹에 사용할 유니크한 아이디                                                                                                                                                     
 impid   | string       | 필수         | 응답에 대한 요청 Imp 오브젝트의 id                                                                                                                                                           
 price   | float        | 필수         | CPM 단위의 입찰 가격                                                                                                                                                                         
 adid    | string       |              | 낙찰시 전송될 광고 ID                                                                                                                                                                        
 nurl    | string       |              | 낙찰시 통보 URL                                                                                                                                                                              
 adm     | string       |              | 광고 마크업                                                                                                                                                                                  
 adomain | string array | 필수         | 광고주 최상위 도메인(광고주 필터링에 사용)                                                                                                                                                   
 bundle  | string       |              | 어플리케이션일 경우 패키지 명(앱광고등)                                                                                                                                                      
 iurl    | string       | 필수         | 광고 컨텐츠 확익을 위한 샘플 이미지 URL                                                                                                                                                      
 cid     | string       | 필수         | 광고 캠페인 ID                                                                                                                                                                               
 crid    | string       | 필수         | 광고물 ID                                                                                                                                                                                    
 cat     | string array |              | 광고물의 컨텐츠 카테고리 목록. IAB OpenRTB Spec 2.3 > 표 5.1 참조                                                                                                                            
 attr    | string array |              | 광고물 속성. OpenRTB Spec 2.3 > 표 5.3 참조                                                                                                                                                  
 w       | integer      |              | 광고물 넓이(pixel)                                                                                                                                                                           
 h       | integer      |              | 광고물 높이(pixel)                                                                                                                                                                           
 ext     | object       |              | 규격 표준을 벗어나는 OpenRTB 주체가 동의한 경우 이 오브젝트로 규격의 유연성을 제공합니다 동의한 경우 이 오브젝트로 규격의 유연성을 제공합니다.
##### 4.2.4 Object: Ext

Bid Object의 Ext. 네이티브 요청시의 응답객체, optouturl(광고 정보 표시 링크 URL)등이 포함 됩니다.

  Name    | Type         | 필수, 기본값 | Description                                                                                                                                                                                  
 :--------|:-------------|:-------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------                                                          
  optouturl   | string       | 권장         | 맞춤형 광고일 경우의 opt-out 설정 url.


#### 4.3 치환(Substitution Macros)

입찰자는 낙찰시 특정 정보(ex: 낙찰 가격)를 전달 받기 위해서 몇가지 치환 매크로를 URL에 삽입합니다.<br> 해당 매크로들에 적합한 데이터가 발견되면 치환하여 낙찰 통보를 합니다. 대체할 값이 없다면 URL에서 삭제됩니다.

 Macro                   | Description                                               
:------------------------|:----------------------------------------------------------
 ${AUCTION_ID}           | BidRequest의 id                                           
 ${AUCTION_ID:B64}       | BidRequest의 id. (Base64로 인코딩)                        
 ${AUCTION_BID_ID}       | BidResponse의 id                                          
 ${AUCTION_BID_ID:B64}   | BidResponse의 id (Base64로 인코딩)                        
 ${AUCTION_IMP_ID}       | 입찰 요청 imp의 ID. Imp의 id                              
 ${AUCTION_IMP_ID:B64}   | 입찰 요청 imp의 ID. Imp의 id (Base64로 인코딩)            
 ${AUCTION_SEAT_ID}      | 입찰자의 입찰자 코드. seatbid의 seat                      
 ${AUCTION_SEAT_ID:B64}  | 입찰자의 입찰자 코드. seatbid의 seat (Base64로 인코딩)    
 ${AUCTION_AD_ID}        | 입찰자가 제공하는 광고의 ID bid의 adid.                   
 ${AUCTION_AD_ID:B64}    | 입찰자가 제공하는 광고의 ID bid의 adid. (Base64로 인코딩)
 ${AUCTION_PRICE}        | 낙찰 가격                                                 
 ${AUCTION_PRICE:B64}    | 낙찰 가격 (Base64로 인코딩)                               
 ${AUCTION_CURRENCY}     | 입찰에 사용한 통화                                        
 ${AUCTION_CURRENCY:B64} | 입찰에 사용한 통화 (Base64로 인코딩)                      

### 5. Native 규격

ExelBid Native는 OpenRTB-Native-Ads-Specification 1.0을 기본으로 구성되었습니다. 

#### 5.1 입찰 요청

입찰 요약 규격(상세 정보는 OpenRTB-Native-Ads-Specification 1.0 참조)

<table>
<tr>
  <th>Field</th>
  <th>Scope</th>
  <th>Field</th>
  <th>Scope</th>
  <th>Field</th>
  <th>Scope</th>
</tr>
<tr>
  <td>ver</td>
  <td>string; optional</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td>layout</td>
  <td>integer; recommended</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td>adunit</td>
  <td>integer; recommended</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td>plcmtcnt</td>
  <td>integer; optional</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td>seq</td>
  <td>integer; optional</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td rowspan="16">assets</td>
  <td rowspan="16">array of objects;required</td>
  <td>id</td>
  <td>integer; required</td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td>required</td>
  <td>integer; optional</td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td>title</td>
  <td>object; optional</td>
  <td>len</td>
  <td>integer; required</td>
</tr>
<tr>
  <td>ext</td>
  <td>object; optional</td>
</tr>
<tr>
  <td rowspan="7">img</td>
  <td rowspan="7">object; optional</td>
  <td>type</td>
  <td>integer; optional</td>
</tr>
<tr>
  <td>w</td>
  <td>integer; optional</td>
</tr>
<tr>
  <td>wmin</td>
  <td>integer; recommended</td>
</tr>
<tr>
  <td>h</td>
  <td>integer; optional</td>
</tr>
<tr>
  <td>hmin</td>
  <td>integer; recommended</td>
</tr>
<tr>
  <td>mimes</td>
  <td>array of strings; required</td>
</tr>
<tr>
  <td>ext</td>
  <td>object; optional</td>
</tr>
<tr>
  <td>video</td>
  <td>object; optional</td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td rowspan="3">data</td>
  <td rowspan="3">object; optional</td>
  <td>type</td>
  <td>integer; required</td>
</tr>
<tr>
  <td>len</td>
  <td>integer; optional</td>
</tr>
<tr>
  <td>ext</td>
  <td>object; optional</td>
</tr>
<tr>
  <td>ext</td>
  <td>object; optional</td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td>ext</td>
  <td>object; optional</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
</tr>
</table>

#### 5.2 입찰 응답

입찰 응답 규격(상세 정보는 OpenRTB-Native-Ads-Specification 1.0 참조)<br>
Exelbid에서는 두가지 입찰 옵션 규격을 제공합니다. 기본적으로 adm 필드 안에 serialized string 으로 포함하거나, 또는 bid->ext->native 형식으로 native object는 ext object 아래에 포함합니다.

<table>
<tr>
  <th>Field</th>
  <th>Scope</th>
  <th>Field</th>
  <th>Scope</th>
  <th>Field</th>
  <th>Scope</th>
  <th>Field</th>
  <th>Scope</th>
</tr>
<tr>
  <td rowspan="26">native</td>
  <td rowspan="26">object;required</td>
  <td>ver</td>
  <td>integer; optional</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td rowspan="18">assets</td>
  <td rowspan="18">array of objects;required</td>
  <td>id</td>
  <td>integer; required</td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td>required</td>
  <td>integer; optional</td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td rowspan="2">title</td>
  <td rowspan="2">object; optional</td>
  <td>text</td>
  <td>string; required</td>
</tr>
<tr>
  <td>ext</td>
  <td>object; optional</td>
</tr>
<tr>
  <td rowspan="4">img</td>
  <td rowspan="4">object; optional</td>
  <td>url</td>
  <td>string; required</td>
</tr>
<tr>
  <td>w</td>
  <td>integer; recommended</td>
</tr>
<tr>
  <td>h</td>
  <td>integer; recommended</td>
</tr>
<tr>
  <td>ext</td>
  <td>object; optional</td>
</tr>
<tr>
  <td rowspan="2">video</td>
  <td rowspan="2">object; optional</td>
  <td>vasttag</td>
  <td>string; required</td>
</tr>
<tr>
  <td>ext</td>
  <td>object; optional</td>
</tr>
<tr>
  <td rowspan="3">data</td>
  <td rowspan="3">object; optional</td>
  <td>label</td>
  <td>string; optional</td>
</tr>
<tr>
  <td>value</td>
  <td>string; required</td>
</tr>
<tr>
  <td>ext</td>
  <td>object; optional</td>
</tr>
<tr>
  <td rowspan="4">link</td>
  <td rowspan="4">object; optional</td>
  <td>url</td>
  <td>string; required</td>
</tr>
<tr>
  <td>clicktrackers[]</td>
  <td>array of strings; required</td>
</tr>
<tr>
  <td><strike>fallback</strike></td>
  <td><strike>string; optional</strike></td>
</tr>
<tr>
  <td>ext</td>
  <td>object; optional</td>
</tr>
<tr>
  <td>ext</td>
  <td>object; optional</td>
  <td></td>
  <td></td>
</tr>

<tr>
  <td rowspan="4">link</td>
  <td rowspan="4">object; required</td>
  <td>url</td>
  <td>string; required</td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td>clicktrackers[]</td>
  <td>array of strings; required</td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td><strike>fallback</strike></td>
  <td><strike>string; optional</strike></td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td>ext</td>
  <td>object; optional</td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td>imptrackers[]</td>
  <td>array of strings;optional</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td><strike>jstracker</strike></td>
  <td><strike>string; optional</strike></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
</tr>
<tr>
  <td>ext</td>
  <td>object; optional</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
</tr>
</table>

### 6. 참조 목록표(Enumerated Lists Specification)

*OpenRTB 2.3 Specification 참조*

### 7. 비딩 요청/응답 예제(Bid Request/Response Samples)

#### 7.1 Bid Requests

##### 7.1.1 Example 1 (이미지 광고 요청)

```json
{
  "id": "57a06a911b3f68cd5cacdc46",
  "imp": [
    {
      "id": "1",
      "banner": {
        "w": 320,
        "h": 50,
        "btype": [
          4
        ],
        "battr": [
          1,
          2,
          3,
          4,
          5,
          6,
          7,
          8,
          9,
          10,
          13,
          14,
          15,
          16
        ],
        "pos": 5
      },
      "displaymanager": "exelbid_test",
      "displaymanagerver": "0.0.1",
      "instl": 0,
      "tagid": "c034046cce8edsff5f458b45dbc9ee4925a3f69a4",
      "bidfloor": 0.1,
      "bidfloorcur": "USD",
      "secure": 0
    }
  ],
  "app": {
    "id": "31",
    "name": "demoapp",
    "bundle": "com.demo.sample",
    "cat": [
      "IAB20"
    ],
    "ver": "4.3.0",
    "publisher": {
      "id": "14",
      "name": "exelbid",
      "cat": [
        "IAB3"
      ],
      "domain": "test.exelbid.com"
    }
  },
  "device": {
    "ua": "Mozilla/5.0 (Windows NT 6.2; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36",
    "geo": {
      "lat": 37.51350021362305,
      "lon": 127.02410125732422,
      "country": "KOR"
    },
    "dnt": 0,
    "lmt": 0,
    "ip": "192.168.171.1",
    "devicetype": 1,
    "make": "samsung,SM",
    "model": "samsung,SM",
    "os": "android",
    "osv": "4.3.0",
    "h": 2560,
    "w": 1440,
    "language": "ko-KR,ko;q=0.8,en-US;q=0.6,en;q=0.4",
    "carrier": "450-5",
    "connectiontype": 3
  },
  "user": {},
  "at": 2,
  "tmax": 200,
  "cur": [
    "USD"
  ],
  "bcat": [
    "IAB12"
  ]
}
```

##### 7.1.2 Example 2 (Native 광고 요청)

```json
{
  "app": {
    "bundle": "com.exelbid",
    "cat": [
      "IAB3"
    ],
    "id": "dc852dbcdf944f9f8c80ab3f281fd967",
    "name": "Truecaller - Caller ID & Block",
    "publisher": {
      "id": "34",
      "name": "publisher"
    },
    "ver": "6.41"
  },
  "at": 2,
  "badv": [
    "badoo.com",
    "bigfish.com",
    "callapp.com",
    "holaa.me",
    "ktcs.co.kr",
    "m.pkr.com",
    "mrnumber.com",
    "nap4319.casino.bigfish.com",
    "pkr.com",
    "pkrtech.com",
    "whitepages.com",
    "whoscall.com"
  ],
  "bcat": [
    "IAB14-1",
    "IAB24",
    "IAB25",
    "IAB26",
    "IAB6-7",
    "IAB7-39",
    "IAB8-18",
    "IAB8-5",
    "IAB9-9"
  ],
  "device": {
    "carrier": "405-799",
    "connectiontype": 2,
    "dnt": 1,
    "geo": {
      "country": "KOR",
      "lat": 37.495499,
      "lon": 127.0162
    },
    "h": 1280,
    "ifa": "df58a938-d087-491d-985f-1c42ca3ef0da",
    "ip": "182.58.217.166",
    "js": 1,
    "language": "en",
    "lmt": 1,
    "make": "samsung",
    "model": "SM-E700H",
    "os": "Android",
    "osv": "5.1.1",
    "ua": "Mozilla\/5.0 (Linux; Android 5.1.1; SM-E700H Build\/LMY47X; wv) AppleWebKit\/537.36 (KHTML, like Gecko) Version\/4.0 Chrome\/47.0.2526.100 Mobile Safari\/537.36",
    "w": 720
  },
  "id": "64fcf249-da49-4e58-a5f0-9ffed99529d6",
  "imp": [
    {
      "bidfloor": 0.014,
      "displaymanager": "test",
      "displaymanagerver": "4.2.0",
      "id": "1",
      "instl": 0,
      "native": {
        "battr": [
          3,
          8,
          9,
          10,
          14,
          6
        ],
        "request": "{\"native\":{\"assets\":[{\"id\":1,\"required\":1,\"title\":{\"len\":25}},{\"id\":2,\"img\":{\"hmin\":80,\"type\":1,\"wmin\":80},\"required\":1},{\"id\":3,\"img\":{\"h\":80,\"type\":2,\"w\":80},\"required\":0},{\"id\":4,\"img\":{\"h\":627,\"type\":3,\"w\":1200},\"required\":1},{\"id\":5,\"data\":{\"len\":100,\"type\":1},\"required\":0},{\"id\":6,\"data\":{\"len\":100,\"type\":2},\"required\":1},{\"id\":7,\"data\":{\"len\":100,\"type\":3},\"required\":0},{\"id\":8,\"data\":{\"len\":100,\"type\":4},\"required\":0},{\"id\":9,\"data\":{\"len\":100,\"type\":5},\"required\":0},{\"id\":10,\"data\":{\"len\":100,\"type\":6},\"required\":0},{\"id\":11,\"data\":{\"len\":100,\"type\":7},\"required\":0},{\"id\":12,\"data\":{\"len\":100,\"type\":8},\"required\":0},{\"id\":13,\"data\":{\"len\":100,\"type\":9},\"required\":0},{\"id\":14,\"data\":{\"len\":100,\"type\":10},\"required\":0},{\"id\":15,\"data\":{\"len\":100,\"type\":11},\"required\":0},{\"id\":16,\"data\":{\"len\":15,\"type\":12},\"required\":0}],\"layout\":6}}",
        "ver": "1.0.0.2"
      },
      "tagid": "072df02f86984dc6b50d74b0ad42bb85"
    }
  ]
}
```

#### 7.2 Bid Responses

##### 7.2.1 Example 1 (이미지 광고 응답)

```json
{
  "id": "57a06a911b3f68cd5cacdc46",
  "seatbid": [
    {
      "bid": [
        {
          "id": "57c52635e0012acf8c2a86e9",
          "impid": "1",
          "price": 1,
          "nurl":"http://test.exelbid.com/exelbid/nurl?id=57c52635e0012acf8c2a86e9&price=${AUCTION_PRICE}",
          "adm": "<a href=\"http://test.exelbid.com/exelbid/click?id=57c52635e0012acf8c2a86e9\" target=\"_top\"><img style=\"width:320px;\" src=\"http://st-dev.onnuridmc.com/banner/201603/7dbe91ea14481e617850633c04a6883d.jpg\" alt=\"Advertisement\" /></a>",
          "adomain": [
            "onnuridmc.com"
          ],
          "iurl": "http://test.exelbid.com/banner/201603/7dbe91ea14481e617850633c04a6883d.jpg",
          "cid": "177",
          "crid": "470",
          "cat": [
            "IAB1"
          ],
          "h": 50,
          "w": 320
        }
      ],
      "seat": "xxx",
      "group": 0
    }
  ],
  "cur": "USD"
}
```

##### 7.2.2 Example 2 (Native 광고 응답)

```json
{
  "id": "61dcf249-da49-4e58-a5f0-9ffed99529d6",
  "seatbid": [
    {
      "bid": [
        {
          "id": "57c5468e0c5fb71f81bf6967",
          "impid": "1",
          "price": 0.10000000149011612,
          "nurl": "http://kr-09.cross-target.com/exelbid/nurl?id=57c5468e0c5fb71f81bf6967&cb_click_url=${CLICK_URL}&price=${AUCTION_PRICE}",
          "adomain": [
            "onnuridmc.com"
          ],
          "iurl": "http://test.exelbid.com/banner/201606/23d4472b27bb3795a5c403b5374906e7.jpg",
          "cid": "178",
          "crid": "527",
          "cat": [
            "IAB1"
          ],
          "h": 0,
          "w": 0,
          "adm": "........."
        }
      ],
      "seat": "xxx",
      "group": 0
    }
  ],
  "cur": "USD"
}
```

