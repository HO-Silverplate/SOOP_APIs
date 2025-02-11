# SOOP_APIs

인터넷 방송 플랫폼 SOOP의 API를 개인적으로 정리한 문서입니다.

채팅 API는 [@DOCHIS](https://github.com/DOCHIS)의 [gist](https://gist.github.com/DOCHIS/8095eb6a05586de220c81503b4684b36)와
[@cha2hyun](https://github.com/cha2hyun)의 [게시물](https://cha2hyun.blog/content/projects/%EB%B0%B0%EB%8F%8C%EC%9D%B4%EC%9D%98%EB%8B%B9%EA%B5%AC%EC%83%9D%ED%99%9C/afreecatv-crawling/)을 참고하여 작성하였습니다.

## 채팅

이 [gist](https://gist.github.com/HO-Silverplate/ba18b03d2a45f6825133bbc881e7f2e8)를 참고하세요.

- [서비스타입](./models/chat/servicetype.json)
- [유저플래그](./models/chat/userflag.json)

## 채널 정보

```
GET https://st.sooplive.co.kr/api/get_station_status.php
```

SOOP의 라이브 API는 스트리머의 닉네임을 응답하지 않기 때문에 닉네임을 알기 위해서는 방송국 정보를 요청해야 합니다.

### PARAMS

```json
    "required":{
        "szBjid" : 스트리머의 고유 ID
    },
    
    "optional":{
        "from_api" : 0,
        "mode":"landing",
        "player_type":"html5",
        "stream_type":"common",
    }
```

### RESPONSE

```json
{
    "RESULT": 1,
    "DATA": {
        "station_no": "10722", // 방송국 번호
        "user_id": "ecvhao", // 스트리머 고유 ID
        "user_nick": "우왁굳",  // 스트리머 닉네임
        "station_name": "우왁굳TV",  // 방송국명 
        "station_title": "우왁굳의 게임 방송국", // 방송국 제목..? 어디에도 표출되지 않음
        "broad_start": "2025-02-09 21:02:44",  // 마지막 방송 시작 시간
        "total_broad_time": "44356096", //총 방송시간(초)
        "grade": "0",
        "fan_cnt": "329759",  // 애청자 수
        "total_visit_cnt": "12429304", // 누적 방문 수
        "total_ok_cnt": "3908332", // 누적 UP 수
        "total_view_cnt": "81684001", // 누적 유저
        "today_visit_cnt": "2278", // 오늘 방문 수
        "today_ok_cnt": "0", // 오늘의 up
        "today_fav_cnt": "54", 
        "total_sub_cnt": 8279 // 구독팬 수
    }
}
```

## 로그인

TBU

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
