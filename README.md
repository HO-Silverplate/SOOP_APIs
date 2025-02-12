<!-- omit from toc -->
# SOOP_APIs

개인적 필요를 위해 인터넷 방송 플랫폼 SOOP의 API를 정리한 문서입니다.

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
- [3. 채팅](#3-채팅)
- [4. 채널 정보](#4-채널-정보)
  - [4.1. PARAMS](#41-params)
  - [4.2. RESPONSE](#42-response)
- [5. 방송국 홈](#5-방송국-홈)
  - [5.1. RESPONSE](#51-response)
- [6. 생방송 정보](#6-생방송-정보)
  - [6.1. BODY](#61-body)
  - [6.2. RESPONSE](#62-response)
    - [6.2.1. 방송중이지 않을 경우](#621-방송중이지-않을-경우)
    - [6.2.2. 방송중일 경우 (성공)](#622-방송중일-경우-성공)
    - [6.2.3. 연령 제한 조건이 충족되지 않은 경우](#623-연령-제한-조건이-충족되지-않은-경우)
  - [6.2.4. 프리셋 객체](#624-프리셋-객체)
  - [6.2.5. 구독 파비콘 객체](#625-구독-파비콘-객체)
- [7. VOD](#7-vod)
- [8. 검색](#8-검색)
- [9. 방송국에서 검색](#9-방송국에서-검색)
- [10. 클립](#10-클립)
- [11. 시그니처 이모티콘](#11-시그니처-이모티콘)

---

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

## 3. 채팅

[이 gist](https://gist.github.com/HO-Silverplate/ba18b03d2a45f6825133bbc881e7f2e8)를 참고하세요.  
해당 gist는 [@DOCHIS](https://github.com/DOCHIS)의 [gist](https://gist.github.com/DOCHIS/8095eb6a05586de220c81503b4684b36)와
[@cha2hyun](https://github.com/cha2hyun)의 [게시물](https://cha2hyun.blog/content/projects/%EB%B0%B0%EB%8F%8C%EC%9D%B4%EC%9D%98%EB%8B%B9%EA%B5%AC%EC%83%9D%ED%99%9C/afreecatv-crawling/)을 참고하여 작성되었습니다.
숲의 채팅 서버와 연결하는 방법은 링크된 게시물을 참고하세요.

- [서비스타입.json](./models/chat/servicetype.json)
- [유저플래그.json](./models/chat/userflag.json)

## 4. 채널 정보

```text
GET https://st.sooplive.co.kr/api/get_station_status.php
```

채널의 각종 통계 자료를 반환합니다.

### 4.1. PARAMS

|     KEY     |       VALUE        |  TYPE  | NECESSARY |
| :---------: | :----------------: | :----: | :-------: |
|   szBjid    | 스트리머의 고유 ID | string |    YES    |
|  from_api   |        ???         | number |    NO     |
|    mode     |        ???         | string |    NO     |
| player_type |   플레이어 유형    | string |    NO     |

### 4.2. RESPONSE

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

## 5. 방송국 홈

```
GET chapi.sooplive.co.kr/api/{BJID}/home
```

방송국 홈 메뉴의 정보를 불러오는 API입니다.
해당 요청을 통해 홈 메뉴에 표출되는 추천 VOD, 게시물 등의 정보를 얻을 수 있습니다.

`BJID`는 스트리머의 고유 ID입니다.

### 5.1. RESPONSE

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

## 6. 생방송 정보

```
POST https://live.sooplive.co.kr/afreeca/player_live_api.php
```

해당 요청은 스트리머의 방송 중 여부와 해당 방송의 각종 정보를 불러옵니다.
생방송의 HLS 플레이리스트와 채팅 서버에 접근하기 위해서 필수적인 정보를 제공하기에 자주 쓰이는 요청입니다.

### 6.1. BODY
|     KEY     |       VALUE        |  TYPE  | NECESSARY |
| :---------: | :----------------: | :----: | :-------: |
|     bid     | 스트리머의 고유 ID | string |    YES    |
|  from_api   |        ???         | number |    NO     |
|    mode     |        ???         | string |    NO     |
| player_type |   플레이어 유형    | string |    NO     |

### 6.2. RESPONSE


#### 6.2.1. 방송중이지 않을 경우

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

#### 6.2.2. 방송중일 경우 (성공)


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
|      FTK      | 스트리머 고유 토큰?*** |     array[object]     |
|  TIER1_NICK   |      1티어 구독명      |        string         |
|  TIER2_NICK   |      2티어 구독명      |        string         |

*\* [6.2.4. 프리셋 객체](#624-프리셋-객체) 참고*  
*\*\* 방송 스트림 요청에 사용*  
*\*\*\* 채팅 서버 연결에 사용*  
*\*\*\*\* [6.2.5. 구독 파비콘 객체](#625-구독-파비콘--객체) 참고*

<details>
<summary> <i>전체 응답 객체 보기</i> </summary>
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

#### 6.2.3. 연령 제한 조건이 충족되지 않은 경우

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
### 6.2.4. 프리셋 객체

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

### 6.2.5. 구독 파비콘 객체
```js
{
    "MONTH": 0,
    "FILENAME": "url"
},
```

## 7. VOD

TBU

## 8. 검색

TBU

## 9. 방송국에서 검색

TBU

## 10. 클립

TBU

## 11. 시그니처 이모티콘
