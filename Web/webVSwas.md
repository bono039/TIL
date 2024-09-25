# 웹 서버 vs WAS

> 🔗 출처 : [WebServer와 WAS의 차이](https://gyoogle.dev/blog/web-knowledge/Web%20Server%EC%99%80%20WAS%EC%9D%98%20%EC%B0%A8%EC%9D%B4.html)

<br/>

||웹 서버|WAS|
|:---|:---|:---|
|특징|정적 컨텐츠 제공|동적 컨텐츠 제공|
|종류|Apache, Nginx |Tomcat 등|

<br/>

### 웹 서버가 필요한 이유
웹 서버에서 정적 컨텐츠만 처리하도록 기능 분배를 함으로써 서버 부담을 줄이기 위함

### WAS가 필요한 이유
WAS로 요청에 맞는 데이터를 DB에서 가져와 비즈니스 로직에 맞게 결과를 제공하며 효율적으로 자원 사용 가능
