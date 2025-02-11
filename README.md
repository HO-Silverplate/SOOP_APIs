# SOOP_APIs

개인적 필요를 위해 인터넷 방송 플랫폼 SOOP의 API를 정리한 문서입니다.

## 로그인

```
POST https://login.sooplive.co.kr/app/LoginAction.php
```

SOOP 계정으로 로그인합니다.

### BODY

```js
//필수 인자
"required":{
    "szWork": "login",
    "szUID": "유저 ID (str)",
    "szPassword": "유저 패스워드 (str)"
},

//선택 인자
"optional":{
    "szType": "json", // 권장
    "szScriptVar" : "oLoginRet", // 추가 로그인 인자, oLoginRet는 로그인 유지 인자
    "isSaveId" : false, // 아이디 저장
    "isLoginRetain" : false // 
}
```

### 응답코드

```js
"RESULTS" : {
    "성공" : 1,
    "등록되지 않은 ID": -1,
    // 등록되지 않은 아이디이거나, 아이디 또는 비밀번호를 잘못 입력하셨습니다.
    "아이디/패스워드 오입력": -3,
    // 아이디/패스워드 입력이 잘못되었습니다.
    "정지": -4,
    // 서비스 정지 URL(https://member.sooplive.co.kr/app/pop_black_clear.php)로 이동
    "탈퇴": -5,
    // 회원탈퇴 URL(https://member.sooplive.co.kr/app/user_delete_progress.php)로 이동
    "멤버쉽 변경": -6,
    // 정확한 용도 불명
    "글로벌 SOOP 차단": -7,
    // 영문 SOOP에서 가입한 계정으로는 한국 SOOP 이용이 불가합니다.
    "법정대리인 동의 필요": -8,
    // 안녕하세요. SOOP 입니다. 접속하신 아이디는 만 14세 미만 법정대리인 동의가 필요한 아이디로써
    // 법정대리인 동의를 받지 않는 경우서비스 이용이 불가능합니다.
    // 서비스 이용에 불편함이 없도록 인증 후 서비스 이용 부탁 드립니다.
    "로그인 차단 본인확인": -9,
    // 로그인 차단 본인확인 팝업 오픈
    "대량접속 차단": -10,
    // 서비스 이용에 불편을 드려 죄송합니다.
    // 회원님 아이디의 비정상적인 로그인(대량 접속 등)이 차단되었습니다.
    // 정상 서비스 이용을 원하실 경우 고객센터로 문의하시면 신속하게 처리해 드리겠습니다.
    "2차 로그인 설정됨": -11, // 2차 로그인 페이지로 이동
    "2차 로그인 설정 요구": -12,
    // 2차비번 미설정시 유저 정보 페이지의 "내 정보 보호" 페이지로 이동
    // 2차비밀번호 설정되어 있을 시 2차비번 찾기 페이지로 이동
    "해외 로그인 차단": -33 // 해외로그인 차단 페이지로 이동
}
```

### 응답 예시

```js
// "szType": "json" 일 경우
// 성공
{
    "RESULT": 1
}

// 실패
{
    "RESULT": -33,
    "szUID": "유저 ID (str)"
}

// "szType" 없을 경우
// 성공
{
    "CHANNEL":{
        "RESULT": 1
    }
}

//실패
{
    "CHANNEL" : {
        "RESULT" : -33,
        "szUid" : "유저 ID (str)"
    }
}
```

## 채팅

[이 gist](https://gist.github.com/HO-Silverplate/ba18b03d2a45f6825133bbc881e7f2e8)를 참고하세요.  
해당 gist는 [@DOCHIS](https://github.com/DOCHIS)의 [gist](https://gist.github.com/DOCHIS/8095eb6a05586de220c81503b4684b36)와
[@cha2hyun](https://github.com/cha2hyun)의 [게시물](https://cha2hyun.blog/content/projects/%EB%B0%B0%EB%8F%8C%EC%9D%B4%EC%9D%98%EB%8B%B9%EA%B5%AC%EC%83%9D%ED%99%9C/afreecatv-crawling/)을 참고하여 작성되었습니다.

- [서비스타입.json](./models/chat/servicetype.json)
- [유저플래그.json](./models/chat/userflag.json)

## 채널 정보

```
GET https://st.sooplive.co.kr/api/get_station_status.php
```

SOOP의 라이브 API는 스트리머의 닉네임을 응답하지 않기 때문에 닉네임을 알기 위해서는 방송국 정보를 요청해야 합니다.

### PARAMS

```js
    // 필수 인자
    "required":{
        "szBjid" : "스트리머의 고유 ID (str)"
    },

    // 선택 인자
    "optional":{
        "from_api" : 0,
        "mode":"landing",
        "player_type":"html5",
        "stream_type":"common",
    }
```

### RESPONSE

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

## 방송국 홈

TBU

## 라이브

TBU

## VOD

TBU

## 검색

TBU

## 방송국에서 검색

TBU

## 클립

TBU

## 시그니처 이모티콘
