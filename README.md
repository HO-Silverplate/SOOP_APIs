<!-- omit from toc -->
# SOOP_APIs

개인적 필요로 인터넷 방송 플랫폼 [숲(SOOP)](https://www.sooplive.co.kr/)의 Open API를 정리한 문서입니다.  
공식 레퍼런스 없이 작성한 문서이기 때문에 오류 또는 누락이 존재할 수 있습니다.

<details>
<summary><strong>목차</strong></summary>
<div markdown="1">

- [1. 로그인](#1-로그인)
  - [1.1. BODY](#11-body)
  - [1.2. 응답코드](#12-응답코드)
  - [1.3. 응답 예시](#13-응답-예시)
  - [1.3.1. "szType" : "json"](#131-sztype--json)
    - [1.3.1.1. 로그인 성공](#1311-로그인-성공)
    - [1.3.1.2. 2차 로그인 필요](#1312-2차-로그인-필요)
    - [1.3.1.3. 실패](#1313-실패)
  - [1.3.2. "szType" : null](#132-sztype--null)
    - [1.3.2.1. 성공](#1321-성공)
    - [1.3.2.2. 2차 로그인 필요](#1322-2차-로그인-필요)
    - [1.3.2.3. 실패](#1323-실패)
  - [1.3.3. "szType" : "text"](#133-sztype--text)
- [2. 2차 비밀번호 로그인](#2-2차-비밀번호-로그인)
  - [2.1. BODY](#21-body)
  - [2.2. RESPONSE](#22-response)
- [3. 로그인 상태 확인](#3-로그인-상태-확인)
  - [3.1. RESPONSE](#31-response)
    - [3.1.1. 로그인 상태](#311-로그인-상태)
    - [3.1.2. 비로그인 상태](#312-비로그인-상태)
- [4. 채팅](#4-채팅)
- [5. 채널 정보](#5-채널-정보)
  - [5.1. PARAMS](#51-params)
  - [5.2. RESPONSE](#52-response)
- [6. 방송국 홈](#6-방송국-홈)
  - [6.1. RESPONSE](#61-response)
- [7. 생방송 정보](#7-생방송-정보)
  - [7.1. BODY](#71-body)
  - [7.2. RESPONSE](#72-response)
    - [7.2.1. 방송중이지 않을 경우](#721-방송중이지-않을-경우)
    - [7.2.2. 방송중일 경우 (성공)](#722-방송중일-경우-성공)
    - [7.2.3. 연령 제한 조건이 충족되지 않은 경우](#723-연령-제한-조건이-충족되지-않은-경우)
  - [7.3. DATATYPE](#73-datatype)
    - [7.3.1. 프리셋 객체](#731-프리셋-객체)
    - [7.3.2. 구독 퍼스너콘 객체](#732-구독-퍼스너콘-객체)
- [8. 생방송 플레이리스트 얻기](#8-생방송-플레이리스트-얻기)
  - [8.1. HLS 키 요청](#81-hls-키-요청)
  - [8.2. 스트림 URL 요청](#82-스트림-url-요청)
  - [8.3. 플레이리스트 요청](#83-플레이리스트-요청)
- [9. VOD 정보](#9-vod-정보)
  - [9.1. BODY](#91-body)
  - [9.2. RESPONSE](#92-response)
  - [9.3. DATATYPE](#93-datatype)
    - [9.3.1. FileType](#931-filetype)
    - [9.3.2. VOD Chapter Object](#932-vod-chapter-object)
    - [9.3.3. 구독 퍼스너콘 객체 #2](#933-구독-퍼스너콘-객체-2)
- [10. 통합 검색](#10-통합-검색)
  - [10.1. 공통 파라메터](#101-공통-파라메터)
  - [10.2. 라이브 검색](#102-라이브-검색)
    - [10.2.1. 추가 파라메터](#1021-추가-파라메터)
    - [10.2.2. RESPONSE](#1022-response)
  - [10.3. VOD 검색](#103-vod-검색)
    - [10.3.1. 추가 파라메터](#1031-추가-파라메터)
    - [10.3.2. RESPONSE](#1032-response)
  - [10.4. 게시글 검색](#104-게시글-검색)
    - [10.4.1. 추가 파라메터](#1041-추가-파라메터)
    - [10.4.2. RESPONSE](#1042-response)
  - [10.5. 스트리머 검색](#105-스트리머-검색)
    - [10.5.1. RESPONSE](#1051-response)
  - [10.6. 카테고리 탐색](#106-카테고리-탐색)
    - [10.6.1. 추가 파라메터](#1061-추가-파라메터)
    - [10.6.2. RESPONSE](#1062-response)
- [11. 채널에서 검색](#11-채널에서-검색)
- [12. 클립](#12-클립)
- [13. 시그니처 이모티콘](#13-시그니처-이모티콘)

</div>
</details>

## 1. 로그인

```
POST https://login.sooplive.co.kr/app/LoginAction.php
```

SOOP 계정으로 로그인합니다.

로그인에 성공한 경우 쿠키에 AuthTicket,  BbsSaveTicket, BbsTicket, RDB, UserTicket, isBbs값이 추가됩니다.

이중 "AuthTicket"값은 로그인된 세션을 유지하기 위해 사용됩니다.

### 1.1. BODY

|      KEY      |                  VALUE                  |  TYPE  | NECESSARY |
| :-----------: | :-------------------------------------: | :----: | :-------: |
|    szWork     |                 "login"                 | string |    YES    |
|     szUid     |                 유저 ID                 | string |    YES    |
|  szPassword   |              유저 패스워드              | string |    YES    |
|    szType     |      응답 타입 (json, text, null)       | string |    NO     |
|  szScriptVar  | 추가 인자 (oLoginRet: 로그인 유지 여부) | string |    NO     |
|   isSaveId    |            ID 기억하기 여부             |  bool  |    NO     |
| isLoginRetain |            로그인 유지 여부             |  bool  |    NO     |

### 1.2. 응답코드

| RESULT |         VALUE          |                                     MSG/ACTION                                      |
| :----: | :--------------------: | :---------------------------------------------------------------------------------: |
|   1    |          성공          |                                                                                     |  |
|   -1   |    미등록 ID/오입력    |      "등록되지 않은 아이디이거나, 아이디 또는 비밀번호를 잘못 입력하셨습니다."      |
|   -3   |         오입력         |                      "아이디/패스워드 입력이 잘못되었습니다."                       |
|   -4   |          정지          |    [정지 안내 URL](https://member.sooplive.co.kr/app/pop_black_clear.php)로 이동    |
|   -5   |          탈퇴          |  [회원탈퇴 URL](https://member.sooplive.co.kr/app/user_delete_progress.php)로 이동  |
|   -6   |       회원 변경        |                                                                                     |
|   -7   |      글로벌 차단       |           "영문 SOOP에서 가입한 계정으로는 한국 SOOP 이용이 불가합니다."            |
|   -8   |  법정대리인 동의 필요  |       "접속하신 아이디는 만 14세 미만 법정대리인 동의가 필요한 아이디로써..."       |
|   -9   |    차단된 계정(1회)    |                           로그인 차단 본인확인 팝업 오픈                            |
|  -10   |     대량접속 차단      |        "회원님 아이디의 비정상적인 로그인(대량 접속 등)이 차단되었습니다..."        |
|  -11   | 2차 비밀번호가 설정됨  |                              2차 로그인 페이지로 이동                               |
|  -12   | 2차 비밀번호 설정 유도 | ["내 정보 보호" 페이지](https://member.sooplive.co.kr/app/user_security.php)로 이동 |
|  -33   |    해외 로그인 차단    |                            해외로그인 차단 페이지로 이동                            |

> 응답코드는 응답의 "RESULT" 필드로 전달됩니다.

### 1.3. 응답 예시  

### 1.3.1. "szType" : "json"

#### 1.3.1.1. 로그인 성공

```js
{
    "RESULT": 1
}
```

#### 1.3.1.2. 2차 로그인 필요

```js
{
    "RESULT": -11,
    "SECOND_LOGIN_KEY" :"2차 로그인 키 (str)",
    "LOGIN_RETAIN": "N" / "Y",
    "nLoginOpt": ""
}
```

> 2차 로그인 키의 사용처는 불명입니다.

#### 1.3.1.3. 실패

```js
{
    "RESULT": -33,
    "szUid": "유저 ID (str)"
}
```

### 1.3.2. "szType" : null



#### 1.3.2.1. 성공

```js
{
    "CHANNEL":{
        "RESULT": 1
    }
}
```

#### 1.3.2.2. 2차 로그인 필요

```js
{
    "CHANNEL":{
        "RESULT": -11,
        "SECOND_LOGIN_KEY" :"2차 로그인 키 (str)",
        "LOGIN_RETAIN": "N" / "Y",
        "nLoginOpt": ""
    }
}
```

#### 1.3.2.3. 실패

```js
{
    "CHANNEL" : {
        "RESULT" : -33,
        "szUid" : "유저 ID (str)"
    }
}
```

### 1.3.3. "szType" : "text"

>결과 코드(int)만 반환합니다.

## 2. 2차 비밀번호 로그인

```
POST https://login.sooplive.co.kr/app/LoginAction.php
```

### 2.1. BODY

|      KEY      |                  VALUE                  |  TYPE  | NECESSARY |
| :-----------: | :-------------------------------------: | :----: | :-------: |
|    szWork     |             "second_login"              | string |    YES    |
|     szUid     |                 유저 ID                 | string |    YES    |
|  szPassword   |              2차 패스워드               | string |    YES    |
|    szType     |      응답 타입 (json, text, null)       | string |    NO     |
|  szScriptVar  | 추가 인자 (oLoginRet: 로그인 유지 여부) | string |    NO     |
|   isSaveId    |            ID 기억하기 여부             |  bool  |    NO     |
| isLoginRetain |            로그인 유지 여부             |  bool  |    NO     |

### 2.2. RESPONSE

| RESULT | VALUE |
| :----: | :---: |
|   1    | 성공  |
|   -1   | 실패  |

## 3. 로그인 상태 확인
```
GET https://afevent2.sooplive.co.kr/api/get_private_info.php
```
세션의 로그인 상태와 로그인된 계정의 간단한 정보를 얻을 수 있는 API입니다.

### 3.1. RESPONSE

#### 3.1.1. 로그인 상태
```js
{
    "CHANNEL":{
        "IS_LOGIN":1,
        "LOGIN_ID":"유저 ID",
        "LOGIN_NICK":"유저 닉네임",
        "LOGIN_CHK":"1",
        "LOGIN_EMAIL":"",
        "STATION_NO":"유저 방송국 번호",
        "SET_HOME":1,
        "NOTE_NEW":"0",
        "AD_POINT":0,
        "AD_FRAME":15
    }
}
```
#### 3.1.2. 비로그인 상태
```js
{
    "CHANNEL": {
        "IS_LOGIN": -1,
        "LOGIN_ID": "",
        "LOGIN_NICK": "",
        "STATION_NO": ""
    }
}
```

## 4. 채팅

[이 gist](https://gist.github.com/HO-Silverplate/ba18b03d2a45f6825133bbc881e7f2e8)를 참고하세요.  
해당 gist는 [@DOCHIS](https://github.com/DOCHIS)의 [gist](https://gist.github.com/DOCHIS/8095eb6a05586de220c81503b4684b36)와
[@cha2hyun](https://github.com/cha2hyun)의 [게시물](https://cha2hyun.blog/content/projects/%EB%B0%B0%EB%8F%8C%EC%9D%B4%EC%9D%98%EB%8B%B9%EA%B5%AC%EC%83%9D%ED%99%9C/afreecatv-crawling/)을 참고하여 작성되었습니다.
숲의 채팅 서버와 연결하는 방법은 링크된 게시물을 참고하세요.

- [서비스타입.json](./models/chat/servicetype.json)
- [유저플래그.json](./models/chat/userflag.json)

## 5. 채널 정보

```text
GET https://st.sooplive.co.kr/api/get_station_status.php
```

채널의 각종 통계 자료를 반환합니다.

### 5.1. PARAMS

|     KEY     |       VALUE        |  TYPE  | NECESSARY |
| :---------: | :----------------: | :----: | :-------: |
|   szBjid    | 스트리머의 고유 ID | string |    YES    |
|  from_api   |        ???         | number |    NO     |
|    mode     |        ???         | string |    NO     |
| player_type |   플레이어 유형    | string |    NO     |

### 5.2. RESPONSE

```js
{
    "RESULT": 1,
    "DATA": {
        "station_no": "99999", // 방송국 번호
        "user_id": "ID", // 스트리머 고유 ID
        "user_nick": "닉네임",  // 스트리머 닉네임
        "station_name": "***TV",  // 방송국명 
        "station_title": "***의 방송국", // 방송국 제목, 어디에도 표출되지 않음
        "broad_start": "YYYY-MM-DD HH:MM:SS",  // 마지막 방송 시작 시간
        "total_broad_time": "44356096", //총 방송시간(초)
        "grade": "0", // ???
        "fan_cnt": "329759",  // 애청자 수
        "total_visit_cnt": "12429304", // 누적 방문 수
        "total_ok_cnt": "3908332", // 누적 UP 수
        "total_view_cnt": "81684001", // 누적 유저
        "today_visit_cnt": "2278", // 오늘 방문 수
        "today_ok_cnt": "0", // 오늘의 up
        "today_fav_cnt": "54", // ???
        "total_sub_cnt": 8279 // 구독팬 수
    }
}
```

## 6. 방송국 홈

```
GET chapi.sooplive.co.kr/api/{BJID}/home
```

방송국 홈 메뉴의 정보를 불러오는 API입니다.
해당 요청을 통해 홈 메뉴에 표출되는 추천 VOD, 게시물 등의 정보를 얻을 수 있습니다.

`BJID`는 스트리머의 고유 ID입니다.

### 6.1. RESPONSE

응답의 각 배열들은 다시보기, 게시글, 유저 클립 등 방송국 홈 화면에 표출될 각 컨텐츠들의 오브젝트로 구성됩니다.

컨텐츠 유형별 오브젝트 예시는 [models](./models/) 디렉토리를 참고하세요. (TBU)

```js
{
    "vods": [],
    "boards": [],
    "user_vods": [],
    "user_boards": [],
    "catchstorys": [],
    "playlists": [],
    "sooptore": {
        "broadcast_live": [],
        "curation_product": []
    },
    "banners": [],
    "is_adsence": true,
    "adballoon_event": {
        "end_yn": true,
        "percent": -1
    }
}
```

## 7. 생방송 정보

```
POST https://live.sooplive.co.kr/afreeca/player_live_api.php
```

해당 요청은 스트리머의 방송 중 여부와 해당 방송의 각종 정보를 불러옵니다.
생방송의 HLS 플레이리스트와 채팅 서버에 접근하기 위해서 필수적인 정보를 제공하기에 자주 쓰이는 요청입니다.

### 7.1. BODY
|     KEY     |       VALUE        |  TYPE  | NECESSARY |
| :---------: | :----------------: | :----: | :-------: |
|     bid     | 스트리머의 고유 ID | string |    YES    |
|  from_api   |        ???         | number |    NO     |
|    mode     |        ???         | string |    NO     |
| player_type |   플레이어 유형    | string |    NO     |

### 7.2. RESPONSE


#### 7.2.1. 방송중이지 않을 경우

```js
{
    "CHANNEL": {
        "geo_cc": "KR",
        "geo_rc": "41",
        "acpt_lang": "ko_KR",
        "svc_lang": "ko_KR",
        "ISSP": 0,
        "RESULT": 0
    }
}
```

#### 7.2.2. 방송중일 경우 (성공)


|      KEY      |         VALUE          |         TYPE          |
| :-----------: | :--------------------: | :-------------------: |
|  VIEWPRESET   |  방송의 품질 프리셋*   |     array[object]     |
|      BNO      |   방송의 식별번호**    |        number         |
|    BJNICK     | 해당 스트리머의 닉네임 |        string         |
|     CATE      |   방송 카테고리 번호   |        number         |
|    CHATNO     |   채팅 서버 번호?***   |        number         |
|     BPWD      |   비밀번호 적용 여부   |    string (Y or N)    |
|     TITLE     |      방송의 제목       |        string         |
|      BPS      |  최고품질 비트레이트   |        string         |
|  RESOLUTION   |    최고품질 해상도     |        string         |
|    S1440P     |  1440p 방송 적용여부   |   number (-1 or 1)    |
| CATEGORY_TAGS | 카테고리 태그(푸른색)  |     array[string]     |
|   HASH_TAGS   |    일반 태그(회색)     |     array[string]     |
|     CHPT      |   채팅 관련 변수***    |        string         |
|   CHDOMAIN    |   채팅 서버 주소***    |        string         |
|     BTIME     |   방송 진행 시간(초)   |        number         |
|  PCON_OBJECT  | 구독 개월별 아이콘**** | object[array[object]] |
|      FTK      | 스트리머 고유 토큰?*** |        string         |
|  TIER1_NICK   |      1티어 구독명      |        string         |
|  TIER2_NICK   |      2티어 구독명      |        string         |

>*\* [7.3.1. 프리셋 객체](#731-프리셋-객체) 참고*  
>*\*\* 방송 스트림 요청에 사용*  
>*\*\*\* 채팅 서버 연결에 사용*  
>*\*\*\*\* [7.3.2. 구독 퍼스너콘 객체](#732-구독-퍼스너콘-객체) 참고*

<details>
<summary>전체 응답 객체 보기</summary>
<div markdown="1">

```js
{
    "CHANNEL": {
        "geo_cc": "KR",
        "geo_rc": "41",
        "acpt_lang": "ko_KR",
        "svc_lang": "ko_KR",
        "ISSP": 0,
        "LOWLAYTENCYBJ": 0,
        "VIEWPRESET": [],
        "RESULT": 1,
        "PBNO": "0",
        "BNO": "*******", // int
        "BJID": "*******", // str
        "BJNICK": "******", // str
        "BJGRADE": 1,
        "STNO": "*****", // int
        "ISFAV": "N",
        "CATE": "00040131",
        "CPNO": 0,
        "GRADE": "0",
        "BTYPE": "30",
        "CHATNO": "2620",
        "BPWD": "N",
        "TITLE": "******", // str
        "BPS": "8000",
        "RESOLUTION": "1920x1080",
        "CTIP": "110.10.76.236",
        "CTPT": "17000",
        "VBT": "1",
        "CTUSER": 50000,
        "S1440P": -1,
        "AUTO_HASHTAGS": [],
        "CATEGORY_TAGS": [
            "VRChat"
        ],
        "HASH_TAGS": [],
        "CHIP": "110.10.76.66",
        "CHPT": "8040",
        "CHDOMAIN": "chat-6E0A4C42.sooplive.co.kr",
        "CDN": "gcp_cdn",
        "RMD": "https://livestream-manager.sooplive.co.kr",
        "GWIP": "222.233.54.57",
        "GWPT": "3456",
        "STYPE": "0",
        "ORG": "N",
        "MDPT": "442",
        "BTIME": 12084,
        "DH": 0,
        "WC": 0,
        "PCON_OBJECT": {
            "tier1": [],
            "tier2": []
        },
        "FTK": "{HASH}_{BJID}_281257200_html5_1",
        "BPCBANNER": true,
        "BPCCHATPOPBANNER": true,
        "BPCTIMEBANNER": false,
        "BPCCONNECTBANNER": true,
        "BPCLOADINGBANNER": true,
        "BPCPOSTROLL": "1",
        "BPCPREROLL": "1",
        "MIDROLL": {
            "VALUE": "5",
            "OFFSET_START_TIME": 0,
            "OFFSET_END_TIME": 45
        },
        "PREROLLTAG": "https://reqde.sooplive.co.kr/api/v1/recommend",
        "MIDROLLTAG": "https://reqde.sooplive.co.kr/api/v1/recommend",
        "POSTROLLTAG": "https://reqde.sooplive.co.kr/api/v1/recommend",
        "BJAWARD": false,
        "BJAWARDWATERMARK": false,
        "BJAWARDYEAR": "2025",
        "GEM": false,
        "GEM_LOG": false,
        "CLEAR_MODE_CATE": [
            "ALL"
        ],
        "PLAYTIMINGBUFFER_DURATION": "2",
        "STREAMER_PLAYTIMINGBUFFER_DURATION": "2",
        "MAXBUFFER_DURATION": "15",
        "LOWBUFFER_DURATION": "0.2",
        "PLAYBACKRATEDELTA": "0.03",
        "MAXOVERSEEKDURATION": "7",
        "TIER1_NICK": "tier1",
        "TIER2_NICK": "tier2",
        "EXPOSE_FLAG": 0,
        "SUB_PAY_CNT": 0,
        "SAVVY_ALLOWED": false
    }
}
```
</div>
</details>

#### 7.2.3. 연령 제한 조건이 충족되지 않은 경우

|    KEY     |        VALUE        |  TYPE  |
| :--------: | :-----------------: | :----: |
|   TITLE    |     방송의 제목     | string |
|    CATE    | 방송 카테고리 번호  | number |
| RESOLUTION |   최고품질 해상도   | string |
|    BPS     | 최고품질 비트레이트 | string |
|   BTIME    | 방송 진행 시간(초)  | number |
|   RESULT   |      결과 코드      | number |

```js
{
    "CHANNEL": {
        "geo_cc": "KR",
        "geo_rc": "41",
        "acpt_lang": "ko_KR",
        "svc_lang": "ko_KR",
        "ISSP": 0,
        "TITLE": "********",
        "CATE": "00130000",
        "RESOLUTION": "1280x720",
        "BPS": "2400",
        "BTIME": 63248,
        "RESULT": -6
    }
}
```

### 7.3. DATATYPE

#### 7.3.1. 프리셋 객체
```js
{
    "label": "360p",
    "label_resolution": "360",
    "name": "sd",
    "bps": 500
},
{
    "label": "540p",
    "label_resolution": "540",
    "name": "hd",
    "bps": 1000
},
{
    "label": "720p",
    "label_resolution": "720",
    "name": "hd4k",
    "bps": 4000
},
{
    "label": "1080p",
    "label_resolution": "1080",
    "name": "original",
    "bps": 8000
},
{
    "label": "자동",
    "label_resolution": "",
    "name": "auto",
    "bps": 8000
}
```

#### 7.3.2. 구독 퍼스너콘 객체

```js
{
    "MONTH": 0,
    "FILENAME": "url"
}
```

## 8. 생방송 플레이리스트 얻기

생방송의 HLS 플레이리스트를 얻기 위해서는 다음과 같은 과정을 거쳐야 합니다 : 

1. 방송 정보 요청
2. HLS 키 요청
3. 스트림 URL 요청
4. .M3U8 플레이리스트 요청

단계별로 서술하겠습니다.

먼저 상술한 [방송 정보 API](#7-생방송-정보)에 요청을 보내 정보를 획득합니다.
이때 활용되는 정보는 다음과 같습니다.
|    KEY     |         VALUE         |     TYPE      |
| :--------: | :-------------------: | :-----------: |
|    BJID    |   스트리머의 고유ID   |    string     |
| VIEWPRESET |  방송의 품질 프리셋   | array[object] |
|    BNO     | 방송 세션의 고유 번호 |    number     |
|    CDN     |       CDN 변수        |    string     |


### 8.1. HLS 키 요청

```
POST https://live.sooplive.co.kr/afreeca/player_live_api.php
```
<!-- omit from toc -->
### BODY
|   KEY   |        VALUE        |  TYPE  | NECESSARY |
| :-----: | :-----------------: | :----: | :-------: |
|  type   |        "aid"        | string |    YES    |
|   bno   | 방송 세션 고유 번호 | number |    YES    |
|   pwd   |    방송 비밀번호    | string |    NO     |
| quality |  품질 프리셋 이름   | string |    NO     |

<!-- omit from toc -->
### RESPONSE

```js
{
    "CHANNEL": {
        "RESULT": 1,
        "AID": "{HASH}" //string
    }
}
```

이렇게 얻어진 HLS 키는 이후 플레이리스트를 요청할 때 파라메터로 쓰입니다.  
  
>**이때, "quality"파라메터를 전달해야만 원하는 품질의 플레이리스트를 받을 수 있습니다.**  
>**또한 비밀번호가 설정된 방송의 경우 이때 "pwd"파라메터로 비밀번호를 전달해야 합니다.**


### 8.2. 스트림 URL 요청
```
GET https://livestream-manager.sooplive.co.kr/broad_stream_assign.html
```

<!-- omit from toc -->
### PARAMS

|     KEY     |                        VALUE                        | NECESSARY |
| :---------: | :-------------------------------------------------: | :-------: |
| return_type | [방송 정보 API](#6-생방송-정보)에서 얻어온 CDN 변수 |    YES    |
|  broad_key  |             {BNO}-common-{quality}-hls              |    YES    |

<!-- omit from toc -->
### RESPONSE

```js
{
    "result":"1",
    "view_url":"https://live-global-cdn-v02.sooplive.co.kr/..",
    "stream_status":"complete"
}
```

이 응답에서 얻은 view_url 정보를 이용해 다음 요청을 진행합니다.

### 8.3. 플레이리스트 요청

```
GET {view_url}
```
<!-- omit from toc -->
### PARAMS

|  KEY  |                        VALUE                        | NECESSARY |
| :---: | :-------------------------------------------------: | :-------: |
|  aid  | [7.1. HLS 키 요청](#71-hls-키-요청)에서 얻은 HLS 키 |    YES    |

<!-- omit from toc -->
### RESPONSE

[auth_playlist.m3u8 예시](/models/playlist/auth_playlist.m3u8)

이렇게 얻어진 플레이리스트는 각 2초 길이의 세그먼트 6개를 포함합니다.  
따라서 해당 요청을 주기적으로 반복하여 플레이리스트를 갱신해야 합니다.

## 9. VOD 정보
```
POST https://api.m.sooplive.co.kr/station/video/a/view 
```

VOD의 각종 정보와 HLS 플레이리스트 파일을 얻을 수 있는 API입니다.


### 9.1. BODY

|   KEY    |    VALUE     |  TYPE  | NECESSARY |
| :------: | :----------: | :----: | :-------: |
| nTitleNo | VOD 고유번호 | number |    YES    |

VOD 고유번호는 VOD 플레이어 URL의 마지막 요소 또는 검색 API의 응답에서 얻어낼 수 있습니다.

### 9.2. RESPONSE

해당 API의 응답은 크게 "result" 필드와 "data" 객체로 구성됩니다.  
"data"필드에 모든 정보가 포함되어 있으므로 "data"객체의 주요 필드만 다루겠습니다.

|           KEY            |                               VALUE                                |     TYPE      |
| :----------------------: | :----------------------------------------------------------------: | :-----------: |
|        writer_id         |                        VOD 게시자의 고유ID                         |    string     |
|          title           |                         VOD의 부분적 제목*                         |    string     |
|          bj_id           |                  VOD 저작권자(스트리머)의 고유ID                   |    string     |
|        bbs_title         |                 VOD 유형(방송 다시보기, Catch 등)                  |    string     |
|        full_title        |                          전체 VOD의 제목*                          |    string     |
|   total_file_duration    |                   전체 VOD의 총 길이(밀리세컨드)                   |    number     |
|         file_bps         |                       VOD의 최고 비트레이트                        |    number     |
|     file_resolution      |                         VOD의 최고 해상도                          |    string     |
|        file_type         |                     [VOD 유형](#931-filetype)                      |    string     |
|         hashtags         |                   ","로 연결된 일반 태그들(회색)                   |    string     |
|         view_cnt         |                               조회수                               |    number     |
|        cate_name         |                             카테고리명                             |    string     |
|      category_tags       |                     카테고리 태그(파란색) 배열                     | array[string] |
|         write_tm         |                 {방송 시작일시} ~ {방송 종료일시}                  |    string     |
|    catch_story_scheme    |                          캐치 스토리 URL                           |    string     |
|     catch_story_idx      |                         캐치 스토리 인덱스                         |    number     |
|   catch_story_title_no   |                        캐치 스토리 고유번호                        |    number     |
|          files           |           [VOD 챕터 객체](#932-vod-chapter-object) 배열            | array[object] |
| subscription_personalcon | [구독 퍼스너콘 객체](#933-구독-퍼스너콘-객체-2) 배열(tier1, tier2) | array[object] |


>*\* 챕터가 나뉜 다시보기를 위해 분리된 것으로 보이지만, 왜인지 같은 값을 리턴함.*

<details>
<summary>전체 응답 객체 보기</summary>
<div markdown="1">

```js
{
    "result": 1,
    "data": {
        "station_no": 99999999,
        "bbs_no": 99999999,
        "writer_id": "{게시자 ID}",
        "board_type": 105,
        "notice_yn": 0,
        "share_yn": 1,
        "comment_yn": 1,
        "auth_no": 101,
        "title": "{VOD 제목 1}",
        "bj_id": "{스트리머 ID}",
        "bbs_title": "방송 다시보기",  // 실제 응답
        "display_auth_no": 101,
        "title_no": 99999999,
        "full_title": "{VOD 제목 2}",
        "writer_nick": "{게시자 닉네임}",
        "content": "  ",
        "grade": 0,
        "thumb": "{THUMBNAIL_URL}",
        "flag": "SUCCEED",
        "ucc_type": "21",
        "total_file_duration": 6302250,
        "file_bps": 8000,
        "file_resolution": "1920x1080",
        "file_type": "REVIEW",
        "show_starballoon": "Y",
        "show_chat": "Y",
        "permanence": "Y",
        "enable_gift": 1,
        "broad_idx": 100233063,
        "region_type": 0,
        "water_mark": 1,
        "hashtags": "회색으로,표시되는,VOD,태그입니다",
        "paid_promotion": 0,
        "auto_hashtags": [],
        "memo_cnt": 9,
        "view_cnt": 34148,
        "recommend_cnt": 48,
        "ucc_favorite_cnt": 0,
        "cate_name": "카테고리",
        "category_tags": ["카테고리"],
        "category_tags_en_us": "Category",
        "category_tags_zh_cn": "Category",
        "category_tags_zh_tw": "Category",
        "category_tags_th_th": "Category",
        "category_tags_ja_jp": "Category",
        "write_tm": "2025-02-12 19:52:29 ~ 2025-02-12 21:38:35",
        "write_timestamp": 1739363915,
        "auto_delete_date": "2025-05-13 21:38:35",
        "category": "00370000",
        "vod_category": "00570013",
        "sub_upload_type": "",
        "is_public": 1,
        "is_show_nft": true,
        "is_nft_contents": false,
        "is_ppv": false,
        "is_paid": false,
        "is_deletable": "N",
        "is_best_partner_review": "Y",
        "quickview": "NOT_USED",
        "is_review_clip": "Y",
        "live_total_view": 33542,
        "broad_start": "2025-02-12 19:52:29",
        "broad_no": null,
        "chat_duration": 300,
        "is_modify": "N",
        "videoballoon_cnt": 0,
        "original_vod": null,
        "fullbtn_case": 3,
        "catch_story_scheme": "https://vod.sooplive.co.kr/player/{catch_story_idx}/catchstory?szLocation=vod_player",
        "catch_story_idx": 999999,
        "catch_story_title_no": 99999999,
        "files": [],
        "vod_seek_time": 0,
        "subscribed": false,
        "subscribe_tier_nick": {
            "tier1_nick": "티어1",
            "tier2_nick": "티어2"
        },
        "active_subscription": true,
        "preroll_showyn": true,
        "midroll_showyn": true,
        "midroll_point_second": [
            988,
            1648,
            2308,
            2968,
            3628,
            4258,
            4888,
            5555,
            6223
        ],
        "midroll_no_reason": "custom_data_none",
        "midroll_setting_type": "recommend",
        "uniquePathKey": "574c8406aa4ba*********8e46777_151093773_1739379490",
        "gift_starballoon": true,
        "category_name": "상위 카테고리 > 하위 카테고리",
        "default_bitrate": 4000,
        "default_quality": "hd4k",
        "share": {
            "url": "https://vod.sooplive.co.kr/player/{title_no}",
            "facebook": "{facebook_share_url}",
            "twitter": "{twitter_share_url}",
            "me2day": "http://m.facebook.com/sharer.php?u=https%3A%2F%2Fvod.sooplive.co.kr%2Fplayer%2F{title_no}"
        },
        "is_later_view": false,
        "station_logo": "https://stimg.sooplive.co.kr/LOGO/{BJID}.substr(0,2)/{BJID}/{BJID}.jpg",
        "is_manager": false,
        "manager_list": [],
        "adballoon": {
            "is_premium": false,
            "load": null,
            "icon_url": "",
            "title": "",
            "url": "https://adballoon.sooplive.co.kr/support.php?view=list&broad_system=VOD&broad_no={BNO}&bj_id={BJID}",
            "list_url": "https://adballoon.sooplive.co.kr/support.php?view=list&broad_system=VOD&broad_no={BNO}&bj_id={BJID}",
            "banner_type": "NONE",
            "message": null,
            "user_nick": "닉네임",
            "campaign_no": 0
        },
        "hash_tags": [
            "회색으로",
            "표시되는",
            "VOD",
            "태그입니다"
        ],
        "subscription_personalcon": {
            "tier1": [],
            "tier2": []
        },
        "is_gem": false,
        "adult_status": "pass",
        "media_exception": {
            "code": 1,
            "msg": ""
        },
        "is_reserve": false,
        "lang": "ko_KR",
        "country": "US",
        "is_recommend": false,
        "is_copyright_like": false,
        "clearmode_category": [
            "ALL"
        ]
    }
}
```

</div>
</details>

### 9.3. DATATYPE

>VOD 객체 관련 정보들은 이후 분리하여 이동할 예정입니다.

#### 9.3.1. FileType

|   TYPE   |   VALUE   |
| :------: | :-------: |
|   ALL    |   전체    |
|  REVIEW  | 다시보기  |
|  NORMAL  | 업로드VOD |
|   CLIP   | 유저클립  |
|  CATCH   |   캐치    |
| PLAYLIST | 재생목록  |

#### 9.3.2. VOD Chapter Object

|     KEY      |               VALUE               |     TYPE      |
| :----------: | :-------------------------------: | :-----------: |
|  file_order  |            챕터 번호*             |    string     |
|     file     |      마스터 플레이리스트 url      |    string     |
|  file_start  |          챕터 시작 일시           |    string     |
| quality_info | 품질 프리셋별 플레이리스트와 정보 | array[object] |


>\* 다시보기가 잘릴 경우, 챕터 번호들이 연속적이지 않을 수 있습니다.

<details>
<summary>전체 응답 객체 보기</summary>
<div markdown="1">

```js
{
    "idx": ********,
    "grade": 0,
    "hide": "N",
    "duration": 6232000,
    "file_order": 1,
    "file": "https://vod-archive-global-cdn-z02.sooplive.co.kr/v102/video/_definst_/smil:vod/20250212/464/281288464/REGL_********_281288464_1.smil/playlist.m3u8",
    "file_type": "REVIEW",
    "file_info_key": "20250212_********_281288464_1",
    "file_start": "2025-02-12 19:52:29",
    "mp4_path": "/vod/20250212/464/281288464/REGL_********_281288464_1.mp4",
    "snapshot": "https://videoimg.sooplive.co.kr/php/SnapshotLoad.php?rowKey=20250212_********_281288464_1_t&column=",
    "honeyjam": [
        0
    ],
    "quality": [
        "원본화질",
        "720p",
        "540p"
    ],
    "quality_files": [
        "https://vod-archive-global-cdn-z02.sooplive.co.kr/v102/video/_definst_/smil:vod/20250212/464/281288464/REGL_********_281288464_1.smil/playlist.m3u8",
        "https://vod-archive-global-cdn-z02.sooplive.co.kr/v102/video/_definst_/smil:vod/20250212/464/281288464/REGL_********_281288464_1.smil/playlist.m3u8",
        "https://vod-archive-global-cdn-z02.sooplive.co.kr/v102/video/_definst_/smil:vod/20250212/464/281288464/REGL_********_281288464_1.smil/playlist.m3u8"
    ],
    "quality_info": [
        {
            "label": "자동",
            "resolution": "",
            "bitrate": "0k",
            "file": "https://vod-archive-global-cdn-z02.sooplive.co.kr/v102/video/_definst_/smil:vod/20250212/464/281288464/REGL_********_281288464_1.smil/playlist.m3u8",
            "name": "adaptive"
        },
        {
            "label": "1080p",
            "resolution": "1920x1080",
            "bitrate": "8000k",
            "file": "https://vod-archive-global-cdn-z02.sooplive.co.kr/v102/video/_definst_/mp4:vod/20250212/464/281288464/REGL_********_281288464_1.mp4/playlist.m3u8",
            "name": "original"
        },
        {
            "label": "720p",
            "resolution": "1280x720",
            "bitrate": "4000k",
            "file": "https://vod-archive-global-cdn-z02.sooplive.co.kr/v102/video/_definst_/mp4:vod/20250212/464/281288464/REGL_********_281288464_1_4000k.mp4/playlist.m3u8",
            "name": "hd4k"
        },
        {
            "label": "540p",
            "resolution": "960x540",
            "bitrate": "1000k",
            "file": "https://vod-archive-global-cdn-z02.sooplive.co.kr/v102/video/_definst_/mp4:vod/20250212/464/281288464/REGL_********_281288464_1_1000k.mp4/playlist.m3u8",
            "name": "hd"
        }
    ],
    "radio_url": "https://vod-archive-global-cdn-z02.sooplive.co.kr/v102/video/_definst_/mp4:vod/20250212/464/281288464/REGL_********_281288464_1.mp4/playlist.m3u8",
    "chat": "https://videoimg.sooplive.co.kr/php/ChatLoadSplit.php?rowKey=20250212_********_281288464_1_c"
},
```

</div>
</details>

#### 9.3.3. 구독 퍼스너콘 객체 \#2

```js
{
    "month": 0,
    "file_name": "{image_url}"
},
```

## 10. 통합 검색

통합 검색 API는 숲의 탐색 탭을 통해 접근할 수 있는 API입니다.  
해당 요청의 모드를 변경하여 카테고리, 생방송, VOD 등 덜 정확하지만 다양한 정보를 검색할 수 있습니다.

```
GET https://sch.sooplive.co.kr/api.php
```

### 10.1. 공통 파라메터

통합 검색 API는 각 검색 모드에 따라 사용 가능한 파라메터가 달라지니, 이하의 모드별 추가 파라메터를 참고하시기 바랍니다.


|     KEY     |      VALUE       |  TYPE  | NECESSARY |       DEPENDANCY       |
| :---------: | :--------------: | :----: | :-------: | :--------------------: |
|   keyword   |      검색어      | string |   NO *    |
|      m      |    검색 모드     | string |    NO     |
|   nPageNo   |  페이지 인덱스   | number |    NO     |
|  nListCnt   | 페이지당 결과 수 | number |    NO     |
|   szOrder   |  검색 정렬 방식  | string |    NO     |
| szOrderType |  오름/내림차순   | string |    NO     | "szOrder" : "view_cnt" |

>\* keyword가 주어지지 않아도 API를 호출할 수는 있지만, 항상 [검색결과 없음]()을 반환합니다. (카테고리 탐색 제외) 

<details>
<summary>검색 모드 접기/펼치기</summary>
<div markdown="1">

|     TYPE     |  VALUE   |
| :----------: | :------: |
|  liveSearch  |  생방송  |
|  vodSearch   |   VOD    |
| postsSearch  |  게시글  |
|   bjSearch   | 스트리머 |
| categoryList | 카테고리 |

</div>
</details>

<details>
<summary>정렬 방식 접기/펼치기</summary>

<div markdown = "1">

|       TYPE       |           VALUE            |           DEPENDANCY           |
| :--------------: | :------------------------: | :----------------------------: |
|      score       |           관련성           |
|     view_cnt     | 조회수 OR 참여자수(라이브) |
|     reg_date     |       업로드 날짜순        |
|   broad_start    |         최근 시작          |       "m" : "liveSearch"       |
|    recomm_cnt    |            UP수            |        "m" : "bjSearch"        |
|  total_view_cnt  |       누적 참여자수        |        "m" : "bjSearch"        |
| total_broad_time |       누적 방송시간        |        "m" : "bjSearch"        |
|   favorite_cnt   |          애청자수          |        "m" : "bjSearch"        |
|   fanclub_cnt    |          팬클럽수          |        "m" : "bjSearch"        |
|      prefer      |       선호 카테고리        | "m" : "categoryList", 로그인됨 |

적합하지 않은 유형의 정렬 방식이 파라메터로 주어지면 관련순으로 정렬합니다. (게시글은 조회수순)  
"view_cnt"로 정렬할 경우, "szOrderType" 파라메터로 "asc" 또는 "desc"를 전달하여 각각 오름차순/내림차순으로 정렬할 수 있습니다.

</div>
</details>

### 10.2. 라이브 검색

#### 10.2.1. 추가 파라메터

|    KEY     |         VALUE          |    TYPE     | NECESSARY |
| :--------: | :--------------------: | :---------: | :-------: |
|  isMobile  |  모바일용 응답(추정)   | number[0,1] |    NO     |
| onlyParent | 중계방 관련 설정(추정) | number[0,1] |    NO     |

> 두 파라메터 모두 응답 데이터에는 영향을 미치지 않았지만 "isMobile" 파라메터는 값에 따라 응답 구조를 바꿀 수 있습니다.

#### 10.2.2. RESPONSE

응답의 "REAL_BROAD"필드로 생방송 정보 객체의 배열이 전달됩니다.

|       KEY       |               VALUE                |  TYPE  |
| :-------------: | :--------------------------------: | :----: |
|   broad_title   |             방송 제목              | string |
|    broad_img    |          방송 썸네일 URL           | string |
| broad_cate_name |         현재 방송 카테고리         | string |
|       url       |            라이브 주소             | string |
|   broad_start   | 방송 시작일시(YYYY-MM-DD HH:MM:SS) | string |
|     user_id     |            스트리머 ID             | string |
|    user_nick    |          스트리머 닉네임           | string |
|    broad_no     |          방송의 식별번호           | string |
|   pc_view_cnt   |            PC 시청자수             | string |
| mobile_view_cnt |          모바일 시청자수           | string |
| total_view_cnt  |            총 시청자수             | string |


<details>
<summary>응답 예시 접기/펼치기</summary>
<div markdown ="1">

```js
{
    "RESULT": "1",
    "HAS_MORE_LIST": true,
    "TOTAL_CNT": "846",
    "processTm": 57799,
    "t": "json",
    "logic": "",
    "charset": "UTF-8",
    "REAL_BROAD": [
        {
            "parent_broad_no": "0",
            "is_nr": "",
            "broad_title": "{TITLE}",
            "broad_img": "https://liveimg.sooplive.co.kr/m/{BNO}",
            "banner_href": "",
            "b_broad_title": "{TITLE}",
            "rank": "-",
            "relay_limit": "",
            "station_name": "{STNAME}",
            "m_current_view_cnt": "1712",
            "broad_cate_name": "{CATEGORY}",
            "broad_bps": "8000",
            "best_grade": "1",
            "is_password": "N",
            "club_name": "",
            "broad_start": "2025-02-14 14:59:31",
            "user_id": "{BJID}",
            "broad_no": "{BNO}",
            "club_id": "",
            "_update_date_time": "2025-02-15T00:13:32",
            "broad_notice": "",
            "layer_flag": "",
            "pc_view_cnt": "2240",
            "broad_addbtn": "",
            "total_view_cnt": "3952",
            "af_tags": [
                {
                    "af_tag": "{tag1}",
                    "af_score": "0.001"
                },
                {
                    "af_tag": "{tag2}",
                    "af_score": "0.0047"
                },
                {
                    "af_tag": "{tag3}",
                    "af_score": "0.0104"
                },
                {
                    "af_tag": "{tag4}",
                    "af_score": "0.0112"
                },
                {
                    "af_tag": "{tag5}",
                    "af_score": "0.0215"
                },
                {
                    "af_tag": "{tag6}",
                    "af_score": "0.0396"
                }
            ],
            "asp_code": "0",
            "broad_resolution": "1920x1080",
            "visit_type": "1",
            "banner_flag": "",
            "layer_url": "",
            "auto_hash_tags": [],
            "broad_horizontal": "1",
            "broad_cate_no": "40066",
            "is_pb": "0",
            "banner_url": "",
            "standard_broad_cate_name": "{CATEGORY}",
            "broad_type": "30",
            "hash_tags": [
                "{HASHTAG}"
            ],
            "broad_memo": "",
            "allowed_view_cnt": "100000",
            "relay_total_current_view_cnt": "0",
            "user_nick": "{BJNICK}",
            "url": "http://afreecatv.com/{BJID}",
            "broad_resolution_n": "2073600",
            "broad_grade": "0",
            "service_type": "0",
            "rcount": "",
            "sn_url": "https://liveimg.sooplive.co.kr/m/{BJID}",
            "is_wp": "-1",
            "broad_bps_n": "8000",
            "current_view_cnt": "2240",
            "logo_flag": "Y",
            "relay_m_total_current_view_cnt": "0",
            "bj_gender": "-1",
            "fan_flag": "0",
            "subs_flag": "0",
            "logic": "",
            "sort": "0.005859203/3952",
            "mobile_view_cnt": "1712",
            "orderCnt": "1",
            "category_tags": [
                "{CATEGORY}"
            ],
            "auto_hashtags": [],
            "favorite_flag": "0",
            "visit_broad_type": 1
        },
    ],
    "SCRAP_BROAD": [],
    "from": "new"
}
```

</div>
</details>

### 10.3. VOD 검색

#### 10.3.1. 추가 파라메터

|     KEY     |       VALUE       |  TYPE  | NECESSARY |         DEPENDANCY         |
| :---------: | :---------------: | :----: | :-------: | :------------------------: |
| szFileType  |     VOD 유형      | string |    NO     |                            |
|   szTerm    |     검색 기간     | string |    NO     |                            |
| szStartDate | 검색 범위 시작일* | string |    NO     | "szTerm" : "period_select" |
|  szEndDate  | 검색 범위 종료일* | string |    NO     | "szTerm" : "period_select" |

> \* YYYY-MM-DD 형태의 문자열

<details>
<summary>VOD 유형 범주 접기/펼치기</summary>
<div markdown = "1">

|  TYPE  |     VALUE     |
| :----: | :-----------: |
| REVIEW | 방송 다시보기 |
| NORMAL |  업로드 VOD   |
| CATCH  |     캐치      |
|  CLIP  |     클립      |

</div>
</details>

<details>
<summary>검색 기간 범주 접기/펼치기</summary>
<div markdown = "1">

|     TYPE      |  VALUE   |
| :-----------: | :------: |
|      all      |   전체   |
|     1year     | 최근 1년 |
|    1month     | 최근 1달 |
|     1week     | 최근 1주 |
|     1day      | 최근 1일 |
| period_select | 직접입력 |

</div>
</details>

#### 10.3.2. RESPONSE

응답의 "DATA" 필드로 VOD 정보 객체의 배열이 전달됩니다.


|         KEY          |              VALUE               |     TYPE      |
| :------------------: | :------------------------------: | :-----------: |
|       title_no       |           VOD 식별번호           |    string     |
|       view_cnt       |              조회수              |    string     |
|  vod_category_name   |         메인 카테고리명          |    string     |
|      file_type       |             VOD 유형             |    string     |
|       reg_date       | 업로드 일시(YYYY-MM-DD HH:MM:SS) |    string     |
|       user_id        |          VOD 제작자 ID           |    string     |
|      user_nick       |        VOD 제작자 닉네임         |    string     |
|     original_bj      |   VOD 저작권자(스트리머)의 ID    |    string     |
|    original_nick     | VOD 저작권자(스트리머)의 닉네임  |    string     |
| original_reg_user_id |        출처 VOD 제작자 ID        |    string     |
|     org_title_no     |        출처 VOD 식별번호         |    string     |
|        title         |             VOD 제목             |    string     |
|       duration       |        VOD 길이(HH:MM:SS)        |    string     |
|    thumbnail_path    |            썸네일 URL            |    string     |
|     vod_duration     |           VOD 길이(초)           |    string     |
|         url          |         VOD 플레이어 URL         |    string     |
|    title_history     |        방제 변경 타임라인        | array[object] |

<details>
<summary>타임라인 정보 객체 접기/펼치기</summary>

<div markdown="1">

|       KEY       |        VALUE        |  TYPE  |
| :-------------: | :-----------------: | :----: |
|      title      |        제목         | string |
|    change_tm    |  변경 시점 (Epoch)  | number |
| change_position | 변경 시점(HH:MM:SS) | string |
|  change_second  |    변경 시점(초)    | number |

```js
{
    "view_cnt": 0,
    "unigram_title": "{TITLE}",
    "change_tm": 1739437514,
    "ngram_title": "{TITLE}",
    "change_position": "02:12:22",
    "title": "{TITLE}",
    "change_second": 7942
}
```

</div>
</details>

<details>
<summary>VOD 정보 객체 예시 접기/펼치기</summary>

```js
{
    "title_no": "{TITLE_NO}",
    "view_cnt": "9628",
    "auth": "OPEN_ALL",
    "vod_category_name": "{CATEGORY}",
    "station_no": "999999999",
    "file_resolution": "1920x1080",
    "file_type": "REVIEW",
    "bbs_no": "99999999",
    "memo_cnt": "0",
    "mobile_thumbnail_path": "http://videoimg.sooplive.co.kr/php/SnapshotLoad.php?rowKey=...",
    "station_name": "{ST_NAME}",
    "recomm_cnt": "1",
    "standard_vod_category_name": "{CATEGORY}",
    "hotclip_yn": "0",
    "vod_view_cnt": "175",
    "encoding_type": "2",
    "auto_hashtags": [],
    "reg_date": "2025-02-14 11:04:14",
    "user_id": "{USERID}",
    "grade": "0",
    "_update_date_time": "2025-02-15T00:24:44",
    "status": "1",
    "title": "{TITLE}",
    "content": "  ",
    "duration": "19:11:12",
    "vod_category": "00040001",
    "unigram_user_nick": "{BJNICK}",
    "thumbnail_path": "http://videoimg.sooplive.co.kr/php/SnapshotLoad.php?rowKey=...",
    "category_id": "",
    "hash_tags": [],
    "aftv_score": "0",
    "b_title": "{TITLE}",
    "user_nick": "{BJNICK}",
    "vod_duration": "69072738",
    "rookie": false,
    "url": "https://vod.afreecatv.com/player/{TITLE_NO}",
    "video_type": "ucc",
    "unigram_title": "{TITLE}",
    "ucc_type": "30",
    "ppv": false,
    "category": "00010000",
    "title_history": [
        {
            "view_cnt": 0,
            "unigram_title": "{TITLE_1}",
            "change_tm": 1739437514,
            "ngram_title": "{TITLE_1}",
            "change_position": "02:12:22",
            "title": "{TITLE_1}",
            "change_second": 7942
        },
        {
            "view_cnt": 0,
            "unigram_title": "{TITLE_2}",
            "change_tm": 1739438351,
            "ngram_title": "{TITLE_2}",
            "change_position": "02:26:19",
            "title": "{TITLE_2}",
            "change_second": 8779
        },
        {
            "view_cnt": 0,
            "unigram_title": "{TITLE_3}",
            "change_tm": 1739441067,
            "ngram_title": "{TITLE_3}",
            "change_position": "03:11:35",
            "title": "{TITLE_3}",
            "change_second": 11495
        },
        {
            "view_cnt": 0,
            "unigram_title": "{TITLE_4}",
            "change_tm": 1739441105,
            "ngram_title": "{TITLE_4}",
            "change_position": "03:12:13",
            "title": "{TITLE_4}",
            "change_second": 11533
        },
        {
            "view_cnt": 0,
            "unigram_title": "{TITLE_5}",
            "change_tm": 1739463363,
            "ngram_title": "{TITLE_5}",
            "change_position": "09:23:08",
            "title": "{TITLE_5}",
            "change_second": 33788
        },
        {
            "view_cnt": 0,
            "unigram_title": "{TITLE_6}",
            "change_tm": 1739481534,
            "ngram_title": "{TITLE_6}",
            "change_position": "14:25:58",
            "title": "{TITLE_6}",
            "change_second": 51958
        }
    ],
    "fan_flag": 0,
    "subs_flag": 0,
    "type": "REVIEW",
    "timestamp": 1739498654,
    "webp_path": "",
    "vertical_thumbnail_path": null,
    "vertical_webp_path": "",
    "broad_date": "",
    "original_user_nick": "",
    "original_reg_user_id": "",
    "original_bj": "",
    "category_tags": [
        "{CATEGORY}"
    ],
    "favorite_flag": 0,
    "use_vertical_thumbnail": false
}
```

</details>

<details>
<summary>전체 응답 예시 접기/펼치기</summary>
<div markdown="1">

```js
{
    "RESULT": 1,
    "TOTAL_CNT": 10000,
    "HAS_MORE_LIST": true,
    "FILETYPE_CNT": null,
    "processTm": 0,
    "t": "json",
    "charset": "UTF-8",
    "DATA": [
        {
            "title_no": "{TITLE_NO}",
            "view_cnt": "9628",
            "auth": "OPEN_ALL",
            "vod_category_name": "{CATEGORY}",
            "station_no": "999999999",
            "file_resolution": "1920x1080",
            "file_type": "REVIEW",
            "bbs_no": "99999999",
            "memo_cnt": "0",
            "mobile_thumbnail_path": "http://videoimg.sooplive.co.kr/php/SnapshotLoad.php?rowKey=...",
            "station_name": "{ST_NAME}",
            "recomm_cnt": "1",
            "standard_vod_category_name": "{CATEGORY}",
            "hotclip_yn": "0",
            "vod_view_cnt": "175",
            "encoding_type": "2",
            "auto_hashtags": [],
            "reg_date": "2025-02-14 11:04:14",
            "user_id": "{USERID}",
            "grade": "0",
            "_update_date_time": "2025-02-15T00:24:44",
            "status": "1",
            "title": "{TITLE}",
            "content": "  ",
            "duration": "19:11:12",
            "vod_category": "00040001",
            "unigram_user_nick": "{BJNICK}",
            "thumbnail_path": "http://videoimg.sooplive.co.kr/php/SnapshotLoad.php?rowKey=...",
            "category_id": "",
            "hash_tags": [],
            "aftv_score": "0",
            "b_title": "{TITLE}",
            "user_nick": "{BJNICK}",
            "vod_duration": "69072738",
            "rookie": false,
            "url": "https://vod.afreecatv.com/player/{TITLE_NO}",
            "video_type": "ucc",
            "unigram_title": "{TITLE}",
            "ucc_type": "30",
            "ppv": false,
            "category": "00010000",
            "title_history": [
                {
                    "view_cnt": 0,
                    "unigram_title": "{TITLE_1}",
                    "change_tm": 1739437514,
                    "ngram_title": "{TITLE_1}",
                    "change_position": "02:12:22",
                    "title": "{TITLE_1}",
                    "change_second": 7942
                },
                {
                    "view_cnt": 0,
                    "unigram_title": "{TITLE_2}",
                    "change_tm": 1739438351,
                    "ngram_title": "{TITLE_2}",
                    "change_position": "02:26:19",
                    "title": "{TITLE_2}",
                    "change_second": 8779
                },
                {
                    "view_cnt": 0,
                    "unigram_title": "{TITLE_3}",
                    "change_tm": 1739441067,
                    "ngram_title": "{TITLE_3}",
                    "change_position": "03:11:35",
                    "title": "{TITLE_3}",
                    "change_second": 11495
                },
                {
                    "view_cnt": 0,
                    "unigram_title": "{TITLE_4}",
                    "change_tm": 1739441105,
                    "ngram_title": "{TITLE_4}",
                    "change_position": "03:12:13",
                    "title": "{TITLE_4}",
                    "change_second": 11533
                },
                {
                    "view_cnt": 0,
                    "unigram_title": "{TITLE_5}",
                    "change_tm": 1739463363,
                    "ngram_title": "{TITLE_5}",
                    "change_position": "09:23:08",
                    "title": "{TITLE_5}",
                    "change_second": 33788
                },
                {
                    "view_cnt": 0,
                    "unigram_title": "{TITLE_6}",
                    "change_tm": 1739481534,
                    "ngram_title": "{TITLE_6}",
                    "change_position": "14:25:58",
                    "title": "{TITLE_6}",
                    "change_second": 51958
                }
            ],
            "fan_flag": 0,
            "subs_flag": 0,
            "type": "REVIEW",
            "timestamp": 1739498654,
            "webp_path": "",
            "vertical_thumbnail_path": null,
            "vertical_webp_path": "",
            "broad_date": "",
            "original_user_nick": "",
            "original_reg_user_id": "",
            "original_bj": "",
            "category_tags": [
                "{CATEGORY}"
            ],
            "favorite_flag": 0,
            "use_vertical_thumbnail": false
        },
    ],
    "RELATED_DATA": {
        "VOD": [],
        "BJ": []
    },
    "LATEST_DATA": [],
    "RECOMMEND_DATA": [],
    "CATCH_DATA": [],
    "CATCH_STORY_DATA": [],
    "WORD": "{KEYWORD}",
    "origin_word": "{KEYWORD}"
}
```

</div>
</details>

### 10.4. 게시글 검색

#### 10.4.1. 추가 파라메터

|  KEY   |   VALUE   |  TYPE  | NECESSARY |
| :----: | :-------: | :----: | :-------: |
| szTerm | 검색 기간 | string |    NO     |

<details>
<summary>검색 기간 범주 접기/펼치기</summary>
<div markdown = "1">

|  TYPE  |  VALUE   |
| :----: | :------: |
|  all   |   전체   |
| 1year  | 최근 1년 |
| 1month | 최근 1달 |
| 1week  | 최근 1주 |
|  1day  | 최근 1일 |

</div>
</details>

#### 10.4.2. RESPONSE

응답의 "DATA" 필드로 게시물 정보 객체의 배열이 전달됩니다.

|       KEY       |                VALUE                 |  TYPE  |
| :-------------: | :----------------------------------: | :----: |
|    title_no     |           게시글 고유번호            | number |
| station_user_id |             스트리머 ID              | string |
|    user_nick    |           스트리머 닉네임            | string |
|    reg_date     |   업로드 일시(YYYY-MM-DD HH:MM:SS)   | string |
|     content     |                 본문                 | string |
|    thumbnail    | 썸네일 URL (없다면 빈 문자열로 대체) | string |
|      title      |                 제목                 | string |
|    view_cnt     |                조회수                | number |
|   recomm_cnt    |                 UP수                 | number |
|    memo_cnt     |               댓글 수                | number |
|  station_logo   |     스트리머의 프로필 이미지 URL     | string |
|       url       |          해당 게시물의 URL           | string |

<details>
<summary>전체 응답 예시 접기/펼치기</summary>
<div markdown="1">

```js
{
    "RESULT": 1,
    "TOTAL_CNT": 10000,
    "processTm": 0,
    "t": "json",
    "charset": "UTF-8",
    "DATA": [
        {
            "station_name": "{STNAME}",
            "title_no": {TITLE_NO},
            "photo_cnt": "0",
            "station_user_id": "{BJID}",
            "station_no": {STATION_NO},
            "user_nick": "{BJNICK}",
            "content": "{CONTENT}",
            "reg_date": "2025-02-16 16:28:35",
            "user_id": "{BJID}",
            "bbs_no": 90309005,
            "favorite_flag": 0,
            "thumbnail": "",
            "title": "{TITLE}",
            "view_cnt": 12,
            "recomm_cnt": 3,
            "memo_cnt": 0,
            "station_logo": "https://stimg.sooplive.co.kr/LOGO/{BJID.substr(0,2)}/{BJID}/{BJID}.jpg",
            "url": "https://ch.sooplive.co.kr/{BJID}/post/{TITLE_NO}"
        }, ...
    ],
    "WORD": "{KEYWORD}",
    "origin_word": "{KEYWORD}"
}
```

</div>
</details>

### 10.5. 스트리머 검색

스트리머 검색은 추가 파라메터를 필요로 하지 않습니다.

#### 10.5.1. RESPONSE

|        KEY        |               VALUE               |     TYPE      |
| :---------------: | :-------------------------------: | :-----------: |
|   station_logo    |    스트리머 프로필 이미지 URL     |    string     |
|     user_nick     |          스트리머 닉네임          |    string     |
|      user_id      |            스트리머 ID            |    string     |
|    station_no     |            방송국 번호            |    string     |
| recent_broad_date | 최근방송일시(YYYY-MM-DD HH:MM:SS) |    string     |
|  total_view_cnt   |           누적 시청자수           |    string     |
|   favorite_cnt    |             애청자 수             |    string     |
| total_broad_time  |          총 방송시간(시)          |    number     |
|      synonym      |         연관 검색어(추정)         | array[string] |
|      medals       |               메달                | array[string] |
|     medal_url     |       대표 메달 이미지 URL        |    string     |

<details>
<summary>전체 응답 예시 접기/펼치기</summary>
<div markdown="1">

```js
{
    "RESULT": 1,
    "TOTAL_CNT": 1548,
    "DATA": [
        {
            "station_logo": "https://profile.img.sooplive.co.kr/LOGO/{BJID.substr(0,2)}/{BJID}/{BJID}.jpg",
            "total_view_cnt": "35481894",
            "station_no": "{STATION_NO}",
            "club_yn": "N",
            "recent_broad_date": "2025-02-15 19:09:27",
            "station_title": "",
            "favorite_cnt": "193129",
            "station_name": "{STNAME}",
            "user_nick": "{BJNICK}",
            "rookie": false,
            "ngram_station_name": "{STNAME}",
            "ngram_station_title": "",
            "user_id": "{BJID}",
            "block_state": "A",
            "grade": "1",
            "_update_date_time": "2025-02-16T16:36:12",
            "ranking": "4",
            "fanclub_cnt": "26889",
            "recent_broad_date_time": "2025-02-15 19:09:27",
            "total_broad_time": 1982,
            "born_year": "0",
            "profile_img_file": "",
            "profile_img_flag": false,
            "synonym": [
                "{SYNONYMS}"
            ],
            "profile_img_height": "0",
            "profile_img_width": "0",
            "qna": [],
            "user_age": "",
            "career_award": [],
            "profile_expose": true,
            "medals": [
                "BEST_STREAMER",
                "PARTNER_STREAMER"
            ],
            "medal_url": "https://res.sooplive.co.kr/images/medal/simple/partner_streamer.png",
            "fan_flag": 0,
            "subs_flag": 0,
            "best_streamer": 1
        }
    ],
    "RECOMMEND_DATA": [],
    "WORD": "{KEYWORD}",
    "t": "json",
    "charset": "UTF-8",
    "version": "2.0"
}
```

</div>
</details>

### 10.6. 카테고리 탐색

#### 10.6.1. 추가 파라메터

|    KEY     |           VALUE           |  TYPE  | NECESSARY |
| :--------: | :-----------------------: | :----: | :-------: |
| szPlatform | 클라이언트 플랫폼(추정) * | string |    NO     |
>\* 응답 내용은 변하지 않고 응답 구조만 바뀝니다. (라이브 검색의 "isMobile" 파라메터와 유사함)

카테고리 탐색 시에는 "szKeyword"가 주어지지 않아도 정상적으로 검색이 가능합니다.

#### 10.6.2. RESPONSE

"data"객체의 "list"필드로 카테고리 정보 객체의 배열을 반환합니다.

|      KEY      |                  VALUE                   |     TYPE      |
| :-----------: | :--------------------------------------: | :-----------: |
|  category_no  |            카테고리 고유번호             |    string     |
| category_name |                카테고리명                |    string     |
|   view_cnt    |  현재 해당 카테고리 방송의 총 시청자수   |    number     |
|  fixed_tags   | 카테고리의 상위 범주(포함된 것으로 간주) | array[string] |
|   cate_img    |           카테고리 이미지 URL            |    string     |

<details>
<summary>전체 응답 예시 접기/펼치기</summary>
<div markdown="1">

```js
{
    "result": 1,
    "data": {
        "is_more": true,
        "offset": 2,
        "list": [
            {
                "category_no": "00720000",
                "category_name": "LCK",
                "view_cnt": 98554,
                "fixed_tags": [
                    "MOBA"
                ],
                "cate_img": "https://admin.img.sooplive.co.kr/category_img/2024/10/15/1707670d9156ca223.jpg"
            },
            {
                "category_no": "00040001",
                "category_name": "스타크래프트",
                "view_cnt": 25248,
                "fixed_tags": [
                    "RTS"
                ],
                "cate_img": "https://admin.img.sooplive.co.kr/category_img/2024/10/15/3568670d900e5fe3c.jpg"
            },
        ]
    }
}
```
</div>
</details>

## 11. 채널에서 검색

채널 검색 API는 스트리머의 채널 페이지에서 상단의 "채널 검색" 필드를 통해 검색할 때 호출되는 API입니다.  
특정 스트리머의 컨텐츠를 검색할 때 유용하게 사용됩니다.

스트리머의 생방송에서 생성된 클립과 캐치 또한 검색할 수 있으며, 제목, 내용, 작성자 등 어떤 필드를 대상으로 검색할지 또한 지정할 수 있습니다.


## 12. 클립

TBU

## 13. 시그니처 이모티콘

TBU
