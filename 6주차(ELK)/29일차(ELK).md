## 📅 29일차 (08/16)
### 🔎 ELK _ elastic search, kibana
나는 멍청이다 나는 망청이다 나는 멍청이더

<br><br>
**Grok Debugger** : 로그 데이터를 구문 분석하고 구조화하는 데 사용되는 grok 패턴을 테스트할 수 있는 도구 !
- Sample Data : 분석할 로그 데이터의 샘플을 입력하는 곳
- Grok Pattern : 샘플 데이터를 구문 분석할 Grok 패턴 정의
- Simulate : Grok 패턴이 샘플 데이터에 적용되어 그 결과가 어떻게 구조화되는 지 보여줌
  
![화면 캡처 2024-08-16 172239](https://github.com/user-attachments/assets/bb3a693c-b985-42e7-8968-c9b16a7b0a18)
### sample data 가지고 직접 대시보드 구성해보기 실습 _ bank salary에서 했는데 다시 들어갔더니 사라졌따 ㅠ

```JavaScirpt
### logstash-pipeline-3.conf 파일
input {
    # beats {
        # port => 5044
    # }
    file {
        path => "C:/ITStudy/05_ELK/apachelog.log" # 이 로그 파일 경로에서 가져오기
        start_position => "beginning"
    }
}

filter {
    grok {
        match => { "message" => "%{COMBINEDAPACHELOG}"}
    }

    mutate {
        remove_field => ["host", "@version", "message"]
    }
}

output {
  elasticsearch {
    hosts => "127.0.0.1:9200"
    index => "logs-%{+YYYY.MM.dd}"
  }
}

## --> 이렇게 한 후 Dev tool의 console 실행하면 결과가 나옴
```
<br>

어려웠던 것 , 헷갈리는 개념
--- 
- vs code 활용해서 뭘 하는 게 구냥 다 어렵댜,, 난 바보다
