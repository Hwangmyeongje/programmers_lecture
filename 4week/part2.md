# Node.js + Express(5)
Express는 HTTTP 보다 동작을 빠르게 해준다.


## Map으로 객체 담기
```javascript
let db = new Map();

let notebook = {
  productName: "Notebook",
  price: 200000,
};
let cup = {
  productName: "Cup",
  price: 7000,
};
let chair = {
  productName: "Chair",
  price: 100000,
};
let poster = {
  productName: "Poster",
  price: 200000,
};
db.set(1, notebook);
db.set(2, cup);
db.set(3, chair);
db.set(4, poster);
```
## Express 모듈 꺼내는 방법
```javascript
const express = require("express");
const app = express();
app.listen(3000);
```

## API 보내는 방법
### 1.id 추가 X
```javascript
const express = require("express");
const app = express();

app.listen(3000);

app.get("/:id", function (req, res) {
  let { id } = req.params;
  id = parseInt(id);

  if (db.get(id) == undefined) {
    res.json({
      message: "없는 상품입니다.",
    });
  } else {
    res.json(db.get(id));
  }
});

let db = new Map();

let notebook = {
  productName: "Notebook",
  price: 200000,
};
let cup = {
  productName: "Cup",
  price: 7000,
};
let chair = {
  productName: "Chair",
  price: 100000,
};
let poster = {
  productName: "Poster",
  price: 200000,
};
db.set(1, notebook);
db.set(2, cup);
db.set(3, chair);
db.set(4, poster);

console.log(db);
console.log(db.get(1));
console.log(db.get(2));
console.log(db.get(3));

```

## id 추가해서 보내는 방법
app.get 부분 변경 ->->
```javascript
app.get("/:id", function (req, res) {
  let { id } = req.params;
  id = parseInt(id);

  if (db.get(id) == undefined) {
    res.json({
      message: "없는 상품입니다.",
    });
  } else {
    product = db.get(id);
    product.id = id;
    res.json(product);
  }
});
```
# Express 구조
Node.js를 위한 빠르고 쉬운 **프레임워크**

프레임워크란 구현하는데 필요한 모든 일을 틀 안에서 할 수 있는 것

### 폴더구조
```
├── app.js
├── bin
│   └── www
├── package.json
├── public
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       └── style.css
├── routes
│   ├── index.js
│   └── users.js
└── views
```

express 제너레이터 설치 시 기본 폴더 구조까지 잡아준다.

1. sudo npm install express-generator -g
2. express
3. npm install
4. npm start

## app.js
express 앱의 본체, entry point로 서버 시작시 app.js에서 시작
```javascript
//express 패키지를 호출해 app 변수 객체를 만드는 로직
//만들어진 app 객체에 기능을 하나씩 연결 한다
//app.set으로 설정을 하나씩 세팅

var createError = require('http-errors');
var express = require('express');
var path = require('path');
var cookieParser = require('cookie-parser');
var logger = require('morgan');

var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');

var app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'jade');

//app. use는 미들웨어를 연결하는 부분

app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', indexRouter);
app.use('/users', usersRouter);

// catch 404 and forward to error handler
app.use(function(req, res, next) {
  next(createError(404));
});

// error handler
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;
```
## bin/www
```javascript
#!/usr/bin/env node

/**
 * Module dependencies.
 */

var app = require('../app');
var debug = require('debug')('express-base:server');
var http = require('http');

/**
 * 환경 변수에서 포트를 가져와 Express에 저장.
 */

var port = normalizePort(process.env.PORT || '3000');
app.set('port', port);

/**
 * HTTP 서버 생성.
 */

var server = http.createServer(app);

/**
 * 모든 네트워크 인터페이스에서 제공된 포트에서 수신 대기.
 */

server.listen(port);
server.on('error', onError);
server.on('listening', onListening);

/**
 * 포트를 숫자, 문자열 또는 false로 정규화.
 */

function normalizePort(val) {
  var port = parseInt(val, 10);

  if (isNaN(port)) {
    // named pipe
    return val;
  }

  if (port >= 0) {
    // port number
    return port;
  }

  return false;
}

/**
 * HTTP 서버 "error" 이벤트를 위한 이벤트 리스너.
 */

function onError(error) {
  if (error.syscall !== 'listen') {
    throw error;
  }

  var bind = typeof port === 'string'
    ? 'Pipe ' + port
    : 'Port ' + port;

  // 특정 수신 대기 오류를 친숙한 메시지로 처리
  switch (error.code) {
    case 'EACCES':
      console.error(bind + ' requires elevated privileges');
      process.exit(1);
      break;
    case 'EADDRINUSE':
      console.error(bind + ' is already in use');
      process.exit(1);
      break;
    default:
      throw error;
  }
}

/**
 * HTTP 서버 "listening" 이벤트를 위한 이벤트 리스너.
 */

function onListening() {
  var addr = server.address();
  var bind = typeof addr === 'string'
    ? 'pipe ' + addr
    : 'port ' + addr.port;
  debug('Listening on ' + bind);
}

```
package.json에 있는 것들을 설치하고 싶으면 npm install 입력

### res 파라미터
- res.send(): 문자열로 응답
- res.json(): json 객체로 응답
- res.render(): html 변환 템플릿을 렌더링
- res.sendfile(): 파일 다운로드
