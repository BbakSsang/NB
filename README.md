# NodejsBack

<pre>const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`)
})</pre>

express 모듈을 'express'이름으로 변경하지 못하게 const로 선언한다 
//모듈을 가져올땐 require('module')
<hr>
express를 함수로 호출express()
app이라는 객체에 넣는다
<hr>
app.get -- get이라는 메소드 호출
사용법은
app.get(path,callback[,callback...])경로로 들어왔을때 호출될 함수를 뜻함
()=> === function(){return ㅇㅇ}
새로운 라우팅방식 fsreaddir등등을 대체
<hr>
app.listen3000
3000번포트에 listen하게 되고, 성공시 뒤의 function을 실행


    response.writeHead(200);
    response.end(html);
데이터를 웹으로 보내주는 기존의 코드에서
response.send(html)로 간소 가능

app.get('/page/:pageId)
--params=/:ㅇㅇㅇ <-이부분에 들어간다

## cookie
쿠키란 웹 안에서 보이지 않게 전송되는 사용자 정보 저장이다
1. 세션관리 용도 : 로그인유지, 장바구니 유지, 게임스코어 등 관리하는 용도로 사용
2. 개인화 : 사용자의 선호 언어, 테마 등을 유지하는 용도
3. 트래킹 사용자의 방문통계 등 행동을 기록하고 분석하기 위한 용도

서버측에서 클라이언트에게 보내주는 키-밸류 형식의 데이터
주로 사용되는것은 세션이 종료되어도 유지되는
``Permenent-Cookie
``Max-Age-${초계산}  (상대적인 종료)
``Secure (https로 통신하는 경우에만 전송되는 쿠키)
``HttpOnly (http로 통신하는 경우에만 전송= console로 자바로 탈취를 막을 수 있음)
``Path=/ㅇㅇㅇ (ㅇㅇㅇ안의 경로에서만 동작한다 제어문)
``Domain=ㅇㅇㅇ (ㅇㅇㅇ안의 모든 서브 페이지에서도 동작한다)


## 쿠키의 경로
기본적으로 쿠키는 request.headers.cookie 에 저장되어있다
하지만 이 정보를 해석하기는 복잡하니

var cookie = require('cookie')  <-- npm insatll cookie 를 통하여 사용
이후에 var cookies = cookie.parse(request.headers.cookie)로 저장이 가능하다