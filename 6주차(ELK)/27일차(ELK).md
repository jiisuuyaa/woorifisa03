## 📅 27일차 (08/13)
### 🔎 ELK _ elastic search

<br><br>
- **검색** : match / term 의 두 쿼리 존재
  - match : 쿼리 수행 전 먼저 분석기를 통해 text 분석
  - term :  별도의 분석 작업 수행 X, 입력된 text가 존재하는 문서를 검색
<br><br>
- **match** : match_all, match, match_phrase, query_string
  - match_all : 인덱스의 모든 문서를 검색, 단순히 인덱스에 있는 모든 문서 반환할 때 사용
  - match : 텍스트 기반 필드에서 주어진 검색어와 일치하는 문서 찾기, 'or' 연산을 사용하여 토큰 중 하나라도 일치하면 문서가 반환
  - match_phrase : 정확하게 구문이 일치하는 문서 찾기
  - query_string : 복잡한 쿼리 문법 사용할 수 있는 쿼리(논리연산자, 와일드카드, 범위/부정 검색)

- **bool query** : must, must_not, should, filter
  - must : 꼭 포함
  - must_not : 절대 불포함
  - should : 해당 조건이 있으면 검색 결과에 가산점
  - filter : 조건을 포함하되 점수에는 영향을 미치지 않는 것. 속도가빠름

 
```JavaScript
## ex. 나이가 34살이면서 firstname이 dillard 검색
POST test/_search
{
  "query": {
    "bool": {
      "must": [
        {"match": {
          "age": 34
          }
        },
        {"match":{
          "gender": "f"
          }
        }
      ]
    }
  }
}
```

- 범위 **gte, lte, gt, lt**
- must와 must_not으로 해결 가능한 eq/ne는 range의 속성으로 사용 불가 
- 문자열과 keyword, 벡터에만 검색 점수 부여 

```JavaScript
# 3. must - pink가 들어가되 should - blue가 같이 들어있는 경우 가중치 부여 (blue가 없는 경우는 후순위로 검색)
GET my_bulk/_search
{
  "query": {
    "bool": {
      "must":
        {"match": {
          "message": "pink"
        }}
      ,
      "should": 
        {"match": {
          "message": "blue"
        }}
    }
  }
}

```
- **Tokenizer** (토크나이저)
  - 문장을 term 단위로 하나씩 분리해내는 처리 과정
  - 토큰필터 : 공백을 기준으로 나뉘어진 term들을 하나씩 가공

- **한글 형태소 분석** : nori 플러그인 설치
  - 형태소를 토큰 형태로 분리하고, 두가지 parameter 지원
  - kuromoji, smartcn 설치 !

```JavaScript
# vs code에서 설치함, 설치 후 elasticsearch 종료 후 재시작 해야함
.\bin/elasticsearch-plugin install analysis-nori   
.\bin/elasticsearch-plugin install analysis-kuromoji
.\bin/elasticsearch-plugin install analysis-smartcn
```


```JavaScript
# 1. 인덱스 생성 및 분석기 설정
# 엘라스틱서치에서는 문서를 인덱싱할 때 사용하는 분석기와 검색할 때 사용하는 분석기가 동일해야 적절히 매칭됩니다.
	#  "aws, amazon, cloud",
            # "pc, personal computer, ibm, mac"
            
PUT /my_synonym_index
{
  "settings": {
    "analysis": {
     "filter": {
        "synonym_filter": {
          "type": "synonym",
          "synonyms_path": "analysis/synonym.txt"
        }
      },
      "analyzer": {
        "my_synonym_analyzer": {
          "tokenizer": "standard",
          "filter": [
            "lowercase",
            "synonym_filter"
          ]
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "title": {
        "type": "text",
        "analyzer": "my_synonym_analyzer"
      }
    }
  }
}


# 2. _analyze API를 사용하여 동의어 처리 확인
POST /my_synonym_index/_analyze
{
  "analyzer": "my_synonym_analyzer",
  "text": "car"
}

# 3. 문서 인덱싱
POST /my_synonym_index/_doc/1
{
  "title": "I love my car"
}

# 4. 동의어를 사용한 검색
GET /my_synonym_index/_search
{
  "query": {
    "match": {
      "title": "automobile"
    }
  }
}
```

### Elasticsearch 설정 보기
```JavaScript
PUT normalizer_test
{
  "settings": { # 인덱스의 설정을 정의하는 섹션 _ 여기선 'normalizer' 설정 중!
    "analysis": {
      "normalizer": {
        "my_normalizer": {
          "type": "custom", # 사용자 정의 nomalizer, 사용자가 직접 'filter'를 지정
          "char_filter": [], # char_filter은 문자필터로, 특정 문자나 패턴을 다른 문자로 변환
          "filter": [ # 텍스트 처리하기 위해 사용되는 필터 목록
            "asciifolding", # 아스키 문자가 아닌 것을 아스키 문자로 변환
            "uppercase" # 모든 텍스트를 대문자로 변환, lowercase는 소문자로 !
          ]
        }
      }
    }
  },
  "mappings": { # mappings 설정 _ 인덱스 포함될 필드와 그 필드의 타입을 정의하는 섹션
    "properties": {
      "myNormalizerKeyword": {
        "type": "keyword",
        "normalizer": "my_normalizer" # my_normalizer을 사용해 텍스트 변환 (위에서 정의한 것)
      },
      "lowercaseKeyword": { 
        "type": "keyword",
        "normalizer": "lowercase" # 텍스트를 모두 소문자로 변환
      },
      "defaultKeyword": {
        "type": "keyword"
      }
    }

  }
}
```
### 위 코드 요약
- 'myNormalizerKeyword' 필드는 아스키 폴딩 및 대문자 변환으로 텍스트 저장
- 'lowercaseKeyword' 필드는 텍스트를 모두 소문자로 변환
- 'defaultKeyword' 필드는 텍스트를 변환하지 않고 원래대로 저장
  
- 실습 : 윤동주 '별헤는 밤' 분석하기
  - my_tokenizer 라는 토크나이저 생성, 시를 잘 분석할 수 있는 애널라이저 넣어보기
  - 내가 생각한 방법 : 문장별로 나누기 -> "요,","다." 를 기준으로 나누고 싶음
  
```JavaScript
DELETE my_tokenizer
PUT /my_tokenizer
{
  "settings": {
    "analysis": {
      "tokenizer": {
        "my_tokenizer": {
          "type": "pattern",
          "pattern": "[.]\\n" # 마침표(.) 뒤에 줄바꿈(\n)이 오는 지점에서 나누기
        }
      },
      "filter": {
        "my_pattern_capture": {
          "type": "pattern_capture", # 텍스트의 특정 부분을 캡처하여 토큰으로 변환
          "preserve_original": true, # 원본 텍스트를 유지하며, 캡처된 부분도 토큰으로 변환하여 포함
          "patterns": ["(.*?요,|.*?다.)"] # "요," 또는 "다."로 끝나는 모든 텍스트를 캡처하여 하나의 토큰으로 변환
        }
      },
      "analyzer": {
        "my_analyzer": {
          "type": "custom",
          "tokenizer": "my_tokenizer",
          "filter": [
            "my_pattern_capture"
          ]
        }
      }
    }
  }
}
```

---
어려웠던 것 , 헷갈리는 개념
---
- vs code와 연결하면서 텍스트 분석하는 과정
- tab.....................

