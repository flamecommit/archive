# HTTP/1.0

HTTP/1.0은 기본적으로 한 연결당 하나의 요청을 처리하도록 설계되었습니다. 이는 RTT 증가를 불러오게 되었습니다.

## RTT 증가

![alt text](image.png)

서버로부터 파일을 가져올 때마다 TCP의 3-웨이 핸드셰이크를 계속해서 열어야 하기 때문에 RTT가 증가하는 단점이 있었습니다.

> **RTT**
>
> 패킷이 목적지에 도달하고 나서 다시 출발지로 돌아오기까지 걸리는 시간이며 패킷 왕복 시간

## RTT의 증가를 해결하기 위한 방법

매번 연결할 때마다 RTT가 증가하니 서버에 부담이 많이 가고 사용자 응답 시간이 길어졌습니다. 이를 해결하기 위해 이미지 스플리팅, 코드 압축, 이미지 Base64 인코딩을 사용하곤 했습니다.

### 이미지 스플리팅

많은 이미지를 다운로드 받게 되면 과부하가 걸리기 때문에 많은 이미지가 합쳐 있는 하나의 이미지를 다운로드받고, 이를 기반으로 background-image의 position을 이용하여 이미지를 표기하는 방법입니다.

```css
#icons>li>a {
    background-image: url("icons.png");
    width: 25px;
    display: inline-block;
    height: 25px;
    repeat: no-repeat;
}
#icons>li:nth-child(1)>a {
    background-position: 2px -8px;
}
#icons>li:nth-child(2)>a {
    background-position: -29px -8px;
}
```

위의 코드처럼 하나의 이미지인 icons.png를 기반으로 background-position 속성을 이용해 2개의 이미지를 설정한 것을 볼 수 있습니다.

### 코드 압축

코드 압축은 코드를 압축해서 개행 문자, 빈칸을 없애서 코드의 크기를 최소화하는 방법입니다.

예를 들어 다음과 같은 코드를 

```js
const express = require('express')
const app = express()
const port = 3000
app.get('/', (req, res) => {
    res.send('Hello World!')
})

app.listen(port, () => {
    console.log(`Example app listening on port ${port}`)
})
```

다음 코드로 바꾸는 방법입니다.

```js
const express=require("express"),app=express(),port=3e3;app.get("/",(e,p)=>{p.send("Hello World!")}),app.listen(3e3,()=>{console.log("Example app listening on port 3000")});
```

이렇게 개행 문자, 띄어쓰기 등이 사라져 코드가 압축되면 코드 용량이 줄어듭니다.

### 이미지 Base64 인코딩

이미지 파일을 64진법으로 이루어진 문자열로 인코딩하는 방법입니다. 이 방법을 사용하면 서버와의 연결을 열고 이미지에 대해 서버에 HTTP 요청을 할 필요가 없다는 장점이 있습니다. 하지만 Base64 문자열로 변환할 경우 37% 정도 크기가 더 커지는 단점이 있습니다.

> **인코딩**
>
> 정보의 형태나 형식을 표준화, 보안, 처리 속도 향상, 저장 공간 절약 등을 위해 다른 형태나 형식으로 변환하는 처리 방식