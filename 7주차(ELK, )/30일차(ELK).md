## 📅 30일차 (08/19)
### 🔎 ELK _ Logstash, Filebeat
--- 
주말동안 사라져버린 내 지식

<br><br>
**Filebeat** 
- 데이터들을 서버에서 다른 곳으로 전송하기 위한 오픈소스로 로그 데이터를 수집하여 전달하고 중앙화하기 위해 많이 사용됨
- 지정한 로그파일 또는 위치를 모니터링하고 그 로그 이벤트를 수집해 elasticsearch 또는 logstash 로 전달

```JavaScirpt
# server1라는 폴더에 아래 파일을 fastapi_server1.py라는 이름으로 작성 후 실행
# pip install fastapi uvicorn
from fastapi import FastAPI

app = FastAPI()

@app.get("/deposit/{amount}")
async def deposit(amount: int):
    return f"{amount}원 입금"

@app.get("/withdraw/{amount}")
async def withdraw(amount: int):
    return f"{amount}원 출금"

$ uvicorn flask-server1:app --log-level info  1> "server1.log"
# /docs 주소로 들어가서 확인

%{LOGLEVEL:log_level}:     %{IP:client_ip}:%{NUMBER:client_port} - "%{WORD:http_method} %{URIPATH:request_path} HTTP/%{NUMBER:http_version}" %{NUMBER:response_code} %{WORD:status_message}

```
<br>

```JavaScript
# server2라는 폴더에 아래 파일을 flask_server2.py라는 이름으로 작성 후 실행
# pip install flask
from flask import Flask

app = Flask(__name__)

@app.route('/hello')
def hello():
    return 'hello world!'

@app.route('/bye')
def bye():
    return 'bye world!'

if __name__ == '__main__':
    app.run(debug=True)

$ python flask_server2.py --logger 2> "server2.log"
```

### server1, server2를 localhost로 연결 후 창에 들어가서 새로고침하면서 로그를 쌓아줘야함

### cat API
- compact and aligned text의 약자
- 엘라스틱 서치의 현재 상태를 조회할 수 있는 api

```JavaScirpt
GET _cat/health  # 전반적인 클러스터의 상태 
GET _cat/indices  # 인덱스의 종류와 상태 조회
GET _cat/nodes  # 각 노드의 이름, 상태 조회
GET _cat/shards   # 샤드의 상태 조회
GET _cat/recovery  # 진행 중이거나 완료된 샤드 복구 작업 정보 조회
GET _cat/allocation  # 샤드 할당에 관한 정보 조회
GET _cat/thread_pool  # 각 노드의 스레드 풀 상태 조회
GET _cat/master  # 현재 마스터로 선출된 노드 확인
## 참고로 엘라스틱 서치에서는 #주석 불가 -> //주석 으로 적기
```
어려웠던 것 , 헷갈리는 개념
--- 
- 괄호가 너무 많다아아악
