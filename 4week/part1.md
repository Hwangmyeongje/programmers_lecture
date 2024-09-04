# Node.js + Express(4)
### param으로 URL 받는 방법

```javascript
pp.get("/:nickname", function (req, res) {
  const param = req.params;
  console.log(param.nickname);
  res.json({
    channel: param.nickname,
  });
});
```

### query로 URL 받는 방법
```javascript
app.get("/watch", function (req, res) {
  const q = req.query;
  console.log(q.v);
  console.log(q.t);

  res.json({
    video: q.v,
    timeline: q.t,
  });
});
```

### 자바스크립트 비구조화
객체는 이름을 맞춰줘야한다.
#### 배열 비구조화
```javascript
const array = [1, 2, 3, 4, 5];
const [num2, num3, num5] = array;
//순서대로 넣고 싶다면
//const [, num2, num3, , num5] = array;

console.log(num2);
console.log(num3);
console.log(num5);
```
출력결과

1

2

3

순서대로 넣어준다.

# 객체 생성 및 API 테스트 결과
### 객체 만들기
```javascript
const express = require("express");
const app = express();

// 채널 주소https://www.youtube.com/@shortbox
// 채널 주소https://www.youtube.com/@leestartv
// 영상클릭주소 https://www.youtube.com/watch?v=GL8E4FmzE9A
// 타임라인 주소 https://www.youtube.com/watch?v=GL8E4FmzE9A&t=1415s

let youtuber1 = {
  channelTitle: "이스타",
  sub: "79.5만명",
  videonNum: "1.1만개",
};

let youtuber2 = {
  channelTitle: "침착맥",
  sub: "227만명",
  videonNum: "6.6천개",
};

let youtuber3 = {
  channelTitle: "테오",
  sub: "54.8만명",
  videonNum: "726개",
};

app.get("/:nickname", function (req, res) {
  const { nickname } = req.params;
  if (nickname == "@leestartv") {
    res.json(youtuber1);
  } else if (nickname == "@ChimChakMan_Official") {
    res.json(youtuber2);
  } else if (nickname == "TEO_universe") {
    res.json(youtuber3);
  } else {
    res.json({
      message: "저희가 모르는 유튜버입니다.",
    });//### 예외처리를 해주어야한다 else로 예외처리

  }
});

app.listen(3000);

```
1. 유튜버 객체를 3개 만든다.
2. res.json으로 객체가 가지고 있는 값들을 보낸다.
3. else문을 이용해 예외처리

## 네이밍 케이스
### **kebab-case, snake_case**
ex)폴더 or 파일

### camelCase
ex)변수, 함수

두 번째 단어의 첫 글자는 "대문자"로
