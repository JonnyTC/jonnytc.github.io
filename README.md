## 기존 기능
-[x] 기사 게시
-[x] 기사 댓글
-[x] 기사 및 댓글 페이지 매김
-[x] 포스트 설정 태그
-[x] 기사 검색 기능
-[x] 게시물 및 댓글 좋아요 기능(좋아요 취소 불가:stick_out_tongue_winking_eye:
-[x] 사용자가 클라이언트 및 기타 작업을 개발하는 데 편리한 `json` 형식으로 정보를 출력할 수 있는 블로그 API 인터페이스. 특정 인터페이스 사용법은 설명 하단을 참조하십시오.
-[x] 기사 작성자 및 기사 상태(닫기 또는 열기)에 따라 기사를 필터링할 수 있습니다.다인 심사는 현재 지원하지 않습니다

블로그 자체에는 기사 게시를 위한 인터페이스가 없지만 GitHub의 문제 페이지에서 직접 새로운 문제입니다.

주석 기능은 [Gitment](https://github.com/imsun/gitment)를 참조하여 Gitment의 CSS 스타일을 차용하여 JavaScript 로직을 다시 작성합니다. 주석 기능은 GitHub 문제를 기반으로 Markdown 구문을 지원하고 @ 기능을 지원하며 like 기능을 지원합니다.

GitHub의 각 기사에 대한 레이블을 지정할 수 있습니다.

404 페이지는 GitHub의 자체 404 페이지를 모방합니다.

### GitHub OAuth 앱 신청
[여기](https://github.com/settings/applications/new)를 클릭하여 신청하세요.
응용 프로그램이 완료되면 해당하는 고유한 **client_id** 및 **client_secret**을 얻게 되며 이 두 문자열은 후속 구성에서 사용됩니다.

## 맞춤형 커스터마이징
### 기본 구성
**config.json** 수정:
```js
{
    "이름": "github 사용자 이름",
    "repo": "github reponame",
    "client_id": "여기에 client_id가 있습니다",
    "client_secret": "여기에 client_secret",
    "제목": "제목 추가",
    "지시": "지시 사항 추가",
    "서버 링크": "http://119.23.8.25/gh-oauth-server.php",
    "필터": {
        "creator": "all", //@param: "all" 또는 사용자 이름(예: "imuncle")
        "state": "open" //@param: "open", "close", "all"
    },
    "메뉴": {
        //여기에 메뉴 항목과 URL을 추가합니다.
        //예시:
        //"집": "./",
        //"RSS": "https://rsshub.app/github/issue/imuncle/imuncle.github.io",
        //"내 소개": "content.html?id=41"
    },
    "친구": {
        // 여기에 친구 링크 추가
        //예시:
        //이문클: "https://imuncle.github.io"
    },
    "아이콘": {
        //여기에 바닥글 아이콘을 추가합니다.
        // 점프 링크를 설정하거나 이미지를 표시할 수 있습니다.
        //주형:
        //"아이콘의 제목": {
        // "icon_src": "아이콘 이미지",
        // "href": "점프하려는 링크",
        // "hidden_img": "보여주고 싶은 이미지",
        // "width": hidden_img의 너비, 숫자여야 합니다.(단위: px)
        //}
        //예시:
        //"깃허브": {
        // "icon_src": "이미지/github.svg",
        // "href": "https://github.com/imuncle",
        // "hidden_img": null,
        //'너비': 0
        //}
    }
}
```
자신의 개인 정보를 입력합니다.

옵션|의미
:--:|:--:
name|당신의 GitHub 사용자 이름을 입력하세요
repo|페이지에 해당하는 창고를 채우십시오. 일반적으로: username.github.io
client_id|OAuth APP을 신청할 때 받은 client_id를 입력하세요.
client_secret|OAuth APP 신청 시 받은 client_secret을 입력하세요.
title|개인 웹사이트의 제목을 입력하세요
지침|개인 웹사이트의 소개를 작성하십시오
server_link|서버 주소를 입력하세요. 서버가 없으면 'http://119.23.8.25/gh-oauth-server.php'를 입력하세요.
필터|작성자 및 문제 상태에 따라 필터링할 수 있는 문제 필터링 규칙을 입력합니다.
menu|이름을 입력하고 오른쪽 메뉴에서 링크
친구|웹사이트의 친구 체인을 채우고, 그렇지 않은 경우 비워 두십시오.
아이콘|웹사이트 바닥글에 있는 아이콘 정보를 채우십시오. 그렇지 않은 경우 비워 두십시오.

위의 server_link는 서버의 주소로 사용자의 access_token은 반드시 서버를 통해 접근해야 하므로 자세한 내용은 [본 기사](https://imuncle.github.io/content.html?id=22)를 참고하세요. . 이 서버는 PHP로 작성되었으며 사용자의 access_token 요청만 담당하며 데이터를 저장하지 않습니다. 자세한 내용은 [소스 코드](https://github.com/imuncle/gitblog/blob/master/server/gh-oauth-server.php)를 참조하세요.

서버가 있는 경우 PHP 코드를 사용하여 서버를 직접 구성하고 **server_link**를 서버 주소로 작성할 수 있습니다.

### 동적 입력 구성
웹사이트 홈페이지에 다이내믹 타이핑 효과가 있는데, 여기서 참고한 것은 [type.js](https://github.com/mattboldt/typed.js) 프로젝트이고, 설정 위치는 **index.js에 있습니다. HTML**.

다음 코드를 찾으십시오(끝에서).
```자바스크립트
$("#changerificwordspanid").typed({
    문자열: ["좋은", "행복한", "건강한", "키가 큰"],
    유형속도: 100,
    시작 지연: 10,
    showCursor: 사실,
    셔플: 사실,
    루프:참
});
```
'문자열'을 변경하여 단어를 변경할 수 있습니다. 더 많은 설정 옵션은 [Original Project](https://github.com/mattboldt/typed.js)를 참고하세요.

### 사진 변경
사진은 모두 **images** 폴더에 저장됩니다.

사진 이름|의미
:--:|:--:
404.png|404 페이지
avatar.jpg|웹사이트 아이콘
fish.png|404 페이지
github.svg|GitHub 아이콘
house1.png|404 페이지
house2.png|404 페이지
page_backfround.jpg|홈페이지의 배경
search.svg|오른쪽 상단 모서리에 있는 검색 아이콘
totop.png|오른쪽 하단 모서리에 있는 "맨 위로 돌아가기" 버튼 아이콘

프론트엔드 지식이 없으시다면 사진 변경시 파일명 변경은 하지 않는 것을 권장합니다.

## API 인터페이스
API 인터페이스 구현은 [api.html](https://github.com/imuncle/gitblog/blob/master/api.html)을 참조하고 파일에 액세스하여 정보를 얻고 url 매개변수를 사용하여 지정합니다. 얻은 정보의 내용. 구체적인 사용법은 다음과 같습니다.

### 메뉴 정보 가져오기
```자바스크립트
$.ajax({
    유형:'가져오기',
    헤더: {
        수락: '응용 프로그램/json',
    },
    url:'귀하의 도메인 이름' +'api.html?menu=menu',
    성공: 함수(데이터) {
        //여기에 코드
    }
});
```

반환된 데이터의 형식은 다음과 같습니다.
```json
[
{
"이름": "집",
"URL": "./"
},
{
"이름": "머신 러닝",
"url": "issue_per_label.html?label=AI"
},
{
"이름": "작은 프로젝트",
"url": "issue_per_label.html?label=프로젝트"
},
{
"이름": "RM 대회",
"url": "issue_per_label.html?label=RM"
},
{
"이름": "ROS 학습",
"url": "issue_per_label.html?label=ROS"
},
{
"이름": "가젯",
"url": "issue_per_label.html?label=tools"
},
{
"이름": "웹 개발",
"url": "issue_per_label.html?label=web"
},
{
"이름": "기타",
"url": "issue_per_label.html?label=other"
},
{
"이름": "영감 아이디어",
"url": "https://imuncle.github.io/timeline"
},
{
"이름": "나에 대해",
"url": "content.html?id=41"
}
]
```

### 기사 목록 가져오기
기사 목록을 얻는 모드는 세 가지가 있습니다. 하나는 필터링하지 않은 일반 모드, 다른 하나는 레이블로 필터링되는 레이블 모드, 다른 하나는 검색 내용으로 필터링되는 검색 모드입니다. 세 가지 모드 모두 페이징 모드를 지원합니다.
```자바스크립트
var request_url ='도메인 이름' +'api.html?';
request_url +='page=1'; //일반 모드
request_url +='label=RM&page=1'; //레이블 모드
request_url +='q=자세분석&page=1'; //검색모드
$.ajax({
    유형:'가져오기',
    헤더: {
        수락: '응용 프로그램/json',
    },
    url: request_url,
    성공: 함수(데이터) {
        //여기에 코드
    }
});
```
> 참고: 위 코드의 'page' 매개변수는 모두 선택 사항입니다.
> 반환된 데이터의 형식은 다음과 같습니다.
```json
{
"페이지": 4,
"page_num": 8,
"기사": [
{
"아이디": 48,
"시간": "2019/4/7 23:00:49",
"제목": "STM32 플래시 읽기 및 쓰기",
"저자": "이문클",
"content": "기사의 내용이 너무 많아 생략합니다...",
"레이블": [
{
"이름": "RM"
}
]
},
{
"아이디": 47,
"시간": "2019/4/5 01:58:44",
"제목": "WS2811 드라이버",
"저자": "이문클",
"content": "기사의 내용이 너무 많아 생략합니다...",
"레이블": [
{
"이름": "RM"
}
]
},
{
"아이디": 46,
"시간": "2019/4/1 18:57:58",
"title": "DS18B20 온도 센서 데이터 읽기",
"저자": "이문클",
"content": "기사의 내용이 너무 많아 생략합니다...",
"레이블": [
{
"이름": "기타"
}
]
},
{
"아이디": 45,
"시간": "2019/4/1 18:01:15",
"title": "HAL 라이브러리는 우리 수준의 지연을 실현합니다",
"저자": "이문클",
"content": "기사의 내용이 너무 많아 생략합니다...",
"레이블": [
{
"이름": "기타"
}
]
},
{
"아이디": 44,
"시간": "2019/4/1 10:00:40",
"제목": "MPU9250 6축 알고리즘",
"저자": "이문클",
"content": "기사의 내용이 너무 많아 생략합니다...",
"레이블": [
{
"이름": "RM"
}
]
},
{
"아이디": 43,
"시간": "2019/3/30 09:19:57",
"title": "MATLAB 직렬 통신 GUI 프로그램",
"저자": "이문클",
"content": "기사의 내용이 너무 많아 생략합니다...",
"레이블": [
{
"이름": "기타"
}
]
},
{
"아이디": 42,
"시간": "2019/3/24 12:01:25",
"제목": "웹사이트 검색 기능",
"저자": "이문클",
"content": "기사의 내용이 너무 많아 생략합니다...",
"레이블": [
{
"이름": "웹"
}
]
},
{
"아이디": 40,
"시간": "2019/3/19 15:19:52",
"title": "RM2018의 투쟁",
"저자": "이문클",
"content": "기사 내용이 너무 많아 생략합니다...",
"레이블": [
{
"이름": "RM"
}
]
},
{
"아이디": 39,
"시간": "2019/3/18 18:03:35",
"제목": "MPU9250 태도 분석",
"저자": "이문클",
"content": "기사의 내용이 너무 많아 생략합니다...",
"레이블": [
{
"이름": "RM"
}
]
},
{
"아이디": 38,
"시간": "2019/3/10 19:03:28",
"title": "아름다운 코드 공유 사진 생성",
"저자": "이문클",
"content": "기사의 내용이 너무 많아 생략합니다...",
"레이블": [
{
"이름": "도구"
}
]
}
]
}
```
> 참고: 기본적으로 한 페이지에 10개의 기사가 표시됩니다.

### 기사 콘텐츠 가져오기
기사의 자세한 내용을 얻기 위함입니다. **참고**, **HTML 형식**의 기사 내용과 '기사 목록 가져오기'로 얻은 **마크다운 형식**의 기사 내용입니다. 사용 방법은 다음과 같습니다.
```자바스크립트
$.ajax({
    유형:'가져오기',
    헤더: {
        수락: '응용 프로그램/json',
    },
    url:'귀하의 도메인 이름' +'api.html?id=1',
    성공: 함수(데이터) {
        //여기에 코드
    }
});
```

반환된 데이터의 형식은 다음과 같습니다.
```json
{
"title": "블로그 구축 과정",
"시간": "2019/2/5 16:33:06",
"content": "기사의 내용이 너무 많아 생략합니다...",
"레이블": [
{
"이름": "웹"
}
],
"좋아요": 0
}
```

## 의존성
* [gitment](https://github.com/imsun/gitment)
* [MathJax](https://www.mathjax.org/)
* [제이쿼리](http://www.jquery.org/)
* [부트스트랩](http://www.getbootstrap.com/)
* [type.js](https://github.com/mattboldt/typed.js)
