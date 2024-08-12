## 26일차 (08/12)
### Linux 마무리 & ELK Stack 시작
- 도커, 리눅스가 얼레벌레 아는 상태로 끝나버려서 ELK부터는 꼬옥 정리하면서 매일 복습하기 !!! 야금야금 이전 개념도 복습하기 !!!

<br><br>
-**Elastic stack** : Elasticsearch[분석,저장], Logstash[수집], Kibana[시각화 도구]
  - Elastic search : 엘라스틱서치는 검색엔진, NoSQL 데이터를 저장하여 이들을 검색
  - NoSQL에서 인덱스는 MySQL에서 DB를 의미함
- 1. 인덱스 생성 및 조회
```JavaScript
PUT /my_index ### 'my_index' 라는 이름의 새로운 인덱스 생성
GET /my_index/_search # 'my_index'에서 모든 문서를 검색(처음이라 아직 없을 수 있다)
```

- 2. 문서 추가/조회
```JavaScript
POST /my_index/_doc
{
    "id": "id",
    "title": "제목",
    "description": "내용"
}
# 'my_index'에 새로운 문서를 추기, 이 문서에는 id,title,description 이 포함됨
```

- 3. 문서 삽입
```JavaScript
PUT my_index/_doc/1
{
  "title": "hello, elasticsearch!",
  "content": "elasticsearch content"
}
# 'my_index'에 ID가 '1'인 문서를 추가, 이 문서에는 'title'과 'content'가 포함됨
```

- 4. 문서 조회
```JavaScript
GET my_index/_doc/1
# 'my_index'에서 ID가 '1'인 문서를 조회
```

- 5. 문서 수정
```JavaScript
POST my_index/_update/1
{
  "doc": 
    {
    "title" : "수정할 제목"
    }
  }
# ID가 1인 문서의 'title' 필드를 '수정할 제목'으로 수정
```

- 6. 문서 삭제
```JavaScript
DELETE my_index/_doc/1
# ID가 1인 문서를 삭제

DELETE my_index/
# 전체 인덱스 삭제
```

- 7. 문서 삽입(자동 ID 생성)
```JavaScript
POST my_index/_doc/
{
  "title": "삽입할 제목",
  "content": "삽입할 내용"
}
# 'my_index'에 새로운 문서를 추가 , 엘라스틱서치가 자동으로 id를 생성해줌
```

- 8. 특정조건 가진 문서 조회
```JavaScript
POST my_index/_search
{
  "query": {
    "match": {
      "title": "hello"
    }
  } 
}
# 'title' 필드에 'hello'가 포함된 문서를 검색 !

GET my_index/_search
# 'my_index'에서 모든 문서를 검색
```

- 9. 문서 전체를 수정
```JavaScript
POST my_index/_doc/mz8FRZEBXpMqRqjivLHY
{
  "doc": 
    {
    "title" : "수정할 제목"
    }
  }

# 문서 전체를 새로운 데이터로 교체(_doc/mz8FRZEBXpMqRqjivLHY의 데이터를 바꿈)
```

- 10. 자료형 확인, 맵핑
```JavaScript
GET my_index
# 'my_index' 의 설정과 자료형을 조회

PUT my_index2
{
  "mappings": {
    "properties": {
      "content" :{
        "type" : "text"
      },
      "title" :{
        "type" : "keyword"
      },
      "longField" : {
        "type": "long"
      }
    }
  }
}
# 'my_index2' 라는 새로운 인덱스를 생성, 각각의 필드의 자료형을 명시적으로 지정해줌 !
```

- 11. 문서 검색
```JavaScript
GET my_index2/_search
# 'my_index2'에서 모든 문서 검색
POST my_index2/_search?q="hello"
# 'my_index2'에서 "hello"라는 단어를 포함하는 문서를 검색
POST my_index2/_search?q=title:"world hello"
# 'my_index2'의 'title' 필드에서 "world hello"와 정확히 일치하는 문서를 덤색
```

---
헷갈리는 개념
---
- **Mapping(매핑)** : 문서가 인덱스에 어떻게 저장되는지 정의하는 부분(sql의 자료형과 비슷)
  - 동적매핑 : 엘라스틱 서치가 자동으로 생성하는 맵핑
  - 명시적매핑 : 사용자가 문서의 각 필드에 데이터를 어떠한 형태로 저장할지 설정으로 지정해주는 맵핑
- ✔ 엘라스틱 서치 & 관계형 데이터 베이스
|    엘라스틱서치    |   RDBMS  | 
| :---------------:  | :------: | 
|  인덱스 | 데이터베이스 |
| 타입 | 테이블 | 
|  문서 | 행 |
|  필드 | 열 |
|  매핑 | 스키마 |
