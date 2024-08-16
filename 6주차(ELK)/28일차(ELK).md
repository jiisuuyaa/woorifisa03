## 📅 28일차 (08/14)
### 🔎 ELK _ elastic search, kibana
아파서 못왔다 끄억

<br><br>
**kibana** : 엘라스틱 스택의 관리, 모니터링, 솔루션을 총괄하는 메인 UI
- 데이터 분석과 시각화 툴
- 엘라스틱 관리
- 엘라스틱 중앙 허브

**시각화 기능**
- 디스커버
- 시각화
- 대시보드
  
```JavaScript
# EUR 통화로 결제한 금액의 총합
GET kibana_sample_data_ecommerce/_search
{
  "size": 0, 
  "query": {
    "term": {
      "currency": {
        "value": "EUR"
      }
    }
  },
  "aggs": {
    "my-sum-aggregation-name": {
      "sum": {
        "field": "taxless_total_price"
      }
    }
  }
}
## "size" :  검색 결과에서 실제 문서를 반환하지 않겠다는 뜻
## "query" : 문서 검색 조건 정의
### "term" 쿼리를 사용해 'currency' 필드가 "EUR" 인 문서들만 필터링 !
```

### cardinality - 지정한 필드가 가진 고유한 값의 개수를 계산해 반환

- HyperLogLog++ 알고리즘 을 사용해 추정한 근사값이다.
- 1. 지정된 threshold 기준으로 각 document을 해싱 (규격화된 난수를 만듬)
  2. 앞의 n자리 정도만 겹치는 값들을 '같은 값'이라고 어림짐작
  3. 결과 돌려주기

- 확실한 **cardinality**를 보장받는 방법 -> 전체 document의 수만큼 threshold를 주기
   

어려웠던 것 , 헷갈리는 개념
---
- 집계 -> 해당 결과를 바탕으로 검색
- term 집계는 각 샤드에서 size 개수만큼 term 뽑아 빈도수를 셈
