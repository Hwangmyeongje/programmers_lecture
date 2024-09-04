# Node.js + Express (6)
# postman

## HTTP method
- 생성 POST
  - ex)회원가입 =나 좀 등록해줘
  - Body에 데이터를 숨겨서 보낸다
  - postman으로 확인한다.
- 조회 GET
- 수정 PUT(덮어쓰기) / PATCH(일부 변경)
- 삭제 DELETE

## POST
post를 확인하기 위해서 postman을 사용

바디에 숨겨져서 들어온 데이터를 화면에 뿌려준다.

- app.use(express.json()
  - Express 애플리케이션에서 json 형태의 요청 body를 받아야 한다면 사용
```javascript
app.use(express.json());

app.post("/test", function (req, res) {
  console.log(req.body.message);
  res.send(req.body.message);
});
```
postman 실습링크(https://www.notion.so/3110553029054702ae401160f90e5e37?pvs=4)

# <API 설계>
## =(URL,method)
- 개별 "조회" GET /url:id : id로 map에서 객체를 찾아서, 그 객체의 정보를 뿌려줌
  - req: params.id : map에 저장된 key값을 전달
  - res: map에서 id로 조회해서 전달
 
- 등록 POST /url
  - req: body 신규 정보를 전달
  - res: 보여주고 싶은 거 보여준다. 
## 응답 객체 - response
## 요청 객체 - request
