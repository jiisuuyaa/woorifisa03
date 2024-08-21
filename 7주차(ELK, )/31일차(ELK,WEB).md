## 📅 31일차 (08/20)
### 🔎 ELK - streamlit으로 배포하기 실습 , WEB (html)
--- 
냥
### elasticsearch
**데이터뷰**
- 엘라스틱서치 인덱스에서 데이터소스를 가져오는 것
  - 날짜별, 연도별로 만든 복수의 인덱스에 대한 매핑을 사전에 병합 -> 쿼리 생성, 시각화에 활용
![1](https://github.com/user-attachments/assets/b80c1d56-418d-4381-a190-483839ab1ff3)
- 만들어진지 얼마나 되었는지, 얼마나 자주 쓰이는 데이터인지에 따라 나눈 기준
1. HOT : 자주 불리워짐
2. WARM : 그럭저럭 많이 불리워짐
3. COLD : 거의 안쓰임
4. FROZEN : 저장은 하고 있지만, 별로 불릴 일이 없음

**streamlit-elk**
- 따로 커밋

<br><br>
### WEB
**인터넷** 
- 컴퓨터와 컴퓨터를 연결해서 통신을 주고 받는 것
- 컴퓨터끼리의 네트워크

**웹** 
- 인터넷에 있는 여러 가지 정보
- 컴퓨터끼리의 네트워크

**HTML** 
- 하이퍼-텍스트로 이루어진 소스코드
- `<, >`로 이루어진 텍스트를 마크업 언어라고 합니다.
- info.cern.ch : 최초의 웹사이트

```JavaScirpt
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8"> <!--기본 인코딩셋은 UTF-8-->
    <meta name="viewport" content="width=device-width, initial-scale=1.0"> #
    <title>현재 창의 제목이 됩니다.</title>
</head>
<body>
    공&nbsp; &nbsp;&nbsp;&nbsp;백
    줄<br>
    바<br>
    꿈<br>
    <h1><i></i></h1>

    <ol>
        <li>사과</li>
        <li>바나나</li>
        <li>딸기</li>
    </ol>
</body>
</html>

<!DOCTYPE html>
<html>
<head>
<title>HTML CSS JS</title>
<!-- 내용(화면에 나오지 않는 모든 것)-->
<style> 
    /* 여
 * 러
 * 줄
 * 의
 * 주석CSS styles */
h1 {
font-family: Impact, sans-serif;
/* 한줄만 주석 color: #CE5937; */
}
  
#welcome {
  color: red;
}

.class1 {
  color: red;
}
</style>
</head>
<body>
<!-- 주석: 1개만 특정 위치를 가리켜야 하는 경우 -->
<h1 id="welcome">HTML CSS JS</h1>
<p>Welcome to HTML-CSS-JS.com</p>
 <!-- 주석:여러개의 위치에 같은 속성을 적용하는 경우  -->
<p class="class1">Online HTML, CSS and JavaScript editor 
with instant preview.</p>
  
<p class="class1">두번째</p>

<script>
    // JavaScript
document.getElementById('welcome').innerText += 
" Editors";
/*
 * 여러줄 */

</script>
</body>
</html>
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
- <head> : 눈에 보이지 않는 것을 모아두는 곳 _ 설정할 필요가 없다면 생략가능, body만 있어도 동작한다. 
태그 이름은 대소문자를 구분하지 않지만, 보통 소문자로 작성


```
어려웠던 것 , 헷갈리는 개념
--- 
- 엘라스틱서치 토크나이저 처리하기
- streamlit 사이트 활용성있게 만드는 아이디어
