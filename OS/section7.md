# 보조기억장치
> 다양한 보조기억장치
> 
> RAID의 정의와 종류

<br/>

## 다양한 보조기억장치
### 하드디스크
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcH18NY%2FbtsC31geClI%2FxKv2iN0hz1GsLeQyZ1S0o1%2Fimg.png" alt="clock" width="300"/>

- 자기적 방식으로 데이터 저장
- 플래터 양면 모두 사용
- 구성
    * 헤드, 디스크 암, 스핀들, 플래터
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc2nofG%2FbtsC1pWcypj%2FHElSl2zgQ3j5TMtW34yTrK%2Fimg.png" alt="clock" width="350"/>

- 저장 단위
     * 플래터 (트랙, 섹터)
       
       <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FqRLnD%2FbtsC4cWkVwx%2F5ShqTCtC8EUOhKvIinZKP0%2Fimg.png" alt="clock" width="350"/>
     * 실린더 (여러 겹 플래터 상에서 같은 트랙이 위치한 곳 모아 연결)
       <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbaSsqR%2FbtsC1IVNUQJ%2FiREAjTj0VRkwpY4MOJk8Wk%2Fimg.png" alt="clock" width="350"/>

- 데이터 접근 과정
    1. <code>탐색 시간</code> : 접근하려는 데이터가 저장된 트랙까지 헤드를 이동시키는 시간
       <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fdc16ks%2FbtsC6xyGIKc%2FCZmK1gv9KN5o4yMS9l44M1%2Fimg.png" alt="clock" width="350"/>
    2. <code>회전 지연</code> : 헤드가 있는 곳으로 플래터를 회전시키는 시간
       <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FeT74rP%2FbtsC4uoYUud%2Fr938noHcNck2tyL7Xsv6T1%2Fimg.png" alt="clock" width="350"/>
    3. <code>전송 시간</code> : 하드 디스크와 컴퓨터 간 데이터를 전송하는 시간

<br/>

<br/>


#### 🔗 참고
* [[컴퓨터 공학 기초 강의] 18강. 다양한 보조기억장치(하드 디스크와 플래시 메모리)](https://youtu.be/m2NfFJEvssY?feature=shared)
* [[컴퓨터 공학 기초 강의] 19강. RAID의 정의와 종류](https://youtu.be/lgFmNUG_Atw?feature=shared)
