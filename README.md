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
주로 사용되는것은 세션이 종료되어도 유지되는<br>
``Permenent-Cookie<br>
``Max-Age-${초계산}  (상대적인 종료)<br>
``Secure (https로 통신하는 경우에만 전송되는 쿠키)<br>
``HttpOnly (http로 통신하는 경우에만 전송= console로 자바로 탈취를 막을 수 있음)<br>
``Path=/ㅇㅇㅇ (ㅇㅇㅇ안의 경로에서만 동작한다 제어문)<br>
``Domain=ㅇㅇㅇ (ㅇㅇㅇ안의 모든 서브 페이지에서도 동작한다)<br>


## 쿠키의 경로
기본적으로 쿠키는 request.headers.cookie 에 저장되어있다
하지만 이 정보를 해석하기는 복잡하니

var cookie = require('cookie')  <-- npm insatll cookie 를 통하여 사용<br>
이후에 var cookies = cookie.parse(request.headers.cookie)에 쿠키값들을 저장이 가능하다
<br>
따라서 간단하게 로그인여부를 확인하거나, 방문기록 등을 보려면
request.headers.cookie에 값이 있다면, 최소한 들어와본적 있다는것이고(사이트에서 방문시 쿠키를 뿌린다는 가정하에)
있을때 미리 만들어둔 객체에
ex)cookies = cookie.parse(request.headers.cookie) 로 값을 저장한뒤, <br>이 객체에 있는 cookis.ㅇㅇㅇ(키값)===???(밸류)랑 맞는지, 맞다면 boolean 함수에 true로 확인이 가능토록 할 수 있다.

<pre>
    response.writeHead(200,{
        'Set-Cookie':[
            'yummy_cookie=choco',
            'tasty_cookie=strawberry',
            `PErmanent=coolie; Max-Age=${60*60*24*30}`,
            `Secure=secure; Secure`,
         `Path=path; Path=/cookie`,
         `Domain=Domain; Domain=o2.org`
        ]
    });
    </pre>처럼 객체로 저장이 되는데 key, value 값으로 지정을하여 저장한다

  <hr>

## session

express의 세션으로 npm install express

이후 
npm install express-session
를 통해 설치가 가능하다

##### 모듈 설치
<pre>
var express = require('express')
var parseurl = require('parseurl')
var session = require('express-session')

var app = express()


app.use(session({
  secret: 'keyboard cat', //필수 공유 금지!!
  resave: false, //값이 바뀌기 전까지세션저장소에 값을 저장하지 않는다 보통 false
  saveUninitialized: true // 세션이 필요하기전까지 구동시키지 않는다 dafault : true
}))
</pre>

### session file

세션들을 저장하는 sessions라는 디렉토리가 생기며 이 안에 세션 값들이 저장된다
설치로는
<pre>
npm install session-file-store
이후


var FileStore = require('session-file-store')(session);

그리고 app.use(session())//필요한 세션값? 들을 넣어야함
안에 
  store: new FileStore(), 를 추가한다
</pre>


### sesion의 삭제 <로그아웃에 주로 사용>
req.session.destroy(funtion(err))<-- err는 콜백함수 


## passport

http://www.passportjs.org/

npm install passport
아이디와 비번으로 로그인하기위해서는
npm install passport-local   (로컬 전략이라고 한다, 페북로그인은 다른걸로 )

Authenticate 인증

app.post('/login', //여기로 정보를 보낸다
  passport.authenticate('local', //로컬방식(전략)
                                  { successRedirect: '/', //성공시
                                   failureRedirect: '/login' })); //실패시


#### 사용방법
<pre>
var passport = require('passport')
  , LocalStrategy = require('passport-local').Strategy;

passport.use(new LocalStrategy(
  function(username, password, done) 
  {
    User.findOne({ username: username }, function (err, user) {
      if (err) { return done(err); }
      if (!user) {
        return done(null, false, { message: 'Incorrect username.' });
      }
      if (!user.validPassword(password)) {
        return done(null, false, { message: 'Incorrect password.' });
      }
      return done(null, user);
    });
  }
));
--------
 User.findOne({ username: username }, function (err, user) // 유저데이터를 가져옴
 if(err): 가져오는데 에러가 생기면 done을 호출하며 반환

 if(!user)유저가 없다면, done함수에 두번째 인자로 false를 주며, 3번째 인자로 왜 실패했는지 알려주면 된다

  if (!user.validPassword(password)) 비번이 틀리면 위와 같음

   return done(null, user); 사용자가 있다면 done에 사용자 정보를준다. 

</pre>

passport 사용시 form의 작성요람은
<pre>
<form action="/login" method="post">
    <div>
        <label>Username:</label>
        <input type="text" name="username"/>
    </div>
    <div>
        <label>Password:</label>
        <input type="password" name="password"/>
    </div>
    <div>
        <input type="submit" value="Log In"/>
    </div>
</form></pre> 이다.

미들웨어 역시 필요한데
app.use(passport.initialize());
app.use(passport.session()); 
---
패스포트가 세션을 사용할때에는
pass.port.serializeUser(function(user,done){
  done(null,user.id)
})

pass.port.deserializeUser(function(user,done){
  User.findById(id,function(err,user){
    done(err,user)
  })
}을 통하여 사용