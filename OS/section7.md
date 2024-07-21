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

<br/>

#### 구성
    * 헤드, 디스크 암, 스핀들, 플래터
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc2nofG%2FbtsC1pWcypj%2FHElSl2zgQ3j5TMtW34yTrK%2Fimg.png" alt="clock" width="350"/>

#### 저장 단위
- 플래터 (트랙, 섹터)
       
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FqRLnD%2FbtsC4cWkVwx%2F5ShqTCtC8EUOhKvIinZKP0%2Fimg.png" alt="clock" width="350"/>
- 실린더 (여러 겹 플래터 상에서 같은 트랙이 위치한 곳 모아 연결)
       <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbaSsqR%2FbtsC1IVNUQJ%2FiREAjTj0VRkwpY4MOJk8Wk%2Fimg.png" alt="clock" width="350"/>

#### 데이터 접근 과정
1. <code>탐색 시간</code> : 접근하려는 데이터가 저장된 트랙까지 헤드를 이동시키는 시간
   <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fdc16ks%2FbtsC6xyGIKc%2FCZmK1gv9KN5o4yMS9l44M1%2Fimg.png" alt="clock" width="350"/>
2. <code>회전 지연</code> : 헤드가 있는 곳으로 플래터를 회전시키는 시간

   <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FeT74rP%2FbtsC4uoYUud%2Fr938noHcNck2tyL7Xsv6T1%2Fimg.png" alt="clock" width="350"/>
4. <code>전송 시간</code> : 하드 디스크와 컴퓨터 간 데이터를 전송하는 시간

<br/>

### 플래시 메모리
    전기적으로 데이터를 읽고 쓰는 반도체 기반 저장 장치

#### 셀
-  플래시 메모리에서 데이터를 저장하는 가장 작은 단위
-  모여서 MB > GB > TB가 됨

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FeT74rP%2FbtsC4uoYUud%2Fr938noHcNck2tyL7Xsv6T1%2Fimg.png" alt="clock" width="350"/>

#### 저장 단위
         
    셀 < 페이지 < 블록 < 플레인 < 다이

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FblPEle%2FbtsC6zpKHQk%2FsHnos7IM2PpdDFhlrmQz5k%2Fimg.png" alt="clock" width="350"/>

* 읽기,쓰기 → 페이지 단위, 삭제 → 블록 단위
* 페이지 상태
    1. Free 상태 - 데이터가 없어 새 데이터를 저장할 수 있는 상태
    2. Valid 상태 - 이미 유효한 데이터를 저장하고 있는 상태
    3. Invalid 상태 - 유효하지 않은 데이터 저장한 상태

<br/>

#### 동작 예시
    * 가비지 컬렉션
        1. 유효한 페이지들만 새 블록으로 복사
        2. 기존 블록 삭제해 공간 정리
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FqtYXt%2FbtsCX7aG9eX%2FZ49kTWrDB7V1ufnlkXipQK%2Fimg.png" alt="clock" width="350"/>
<br/>

## RAID의 정의와 종류
    데이터의 안전성/고성능 위해 여러 물리적 보조기억장치를 하나의 논리적 보조기억장치처럼 사용하는 기술

- 하드 디스크와 SSD 사용
<br/>

### RAID 레벨
- 정의 : RAID 구성하는 기술

### 종류
#### 1. RAID 0
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcwVQ1s%2FbtsCWJuqzqm%2FDzSqfvmZIXJoz6YSNJ0xs1%2Fimg.png" alt="clock" width="350"/>

- 데이터 단순히 나눠 저장
- 👍 입출력 속도 향상
- 👎 저장된 정보 안전 X

- <code>스트라입(stripe)</code> : 분산되어 저장된 데이터
- <code>스트라이핑(striping)</code> : 분산하여 저장하는 것

<br/>

#### 2. RAID 1
- 데이터 쓸 때 원본과 복사본 2곳에 씀 (쓰기 속도 느림)
- <code>미러링</code> : 복사본 만드는 방식
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fn95eY%2FbtsC4DTEnXP%2FWo09i9kseCG51aEFoAz75K%2Fimg.png" alt="clock" width="350"/>
<br/>

#### 3. RAID 4
- 완전한 복사본 대신 **패리티 비트** 저장
- 패리티 저장한 장치 이용해 다른 장치들의 오류 검출 및 복구
- 👎 패리티 디스크의 병목
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcWT1xG%2FbtsC4fFxw0Q%2FvQyAIRZCTCX0HmrXqc470k%2Fimg.png" alt="clock" width="350"/>

<br/>

#### 4. RAID 5
- 패리티 정보를 분산해 저장하는 방식
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbpiaix%2FbtsC6qT2yR5%2FegbTHDlxSImnf4Ps2u3PS1%2Fimg.png" alt="clock" width="350"/>

<br/>

#### 5. RAID6
- 두 종류의 패리티 사용
- RAID 5보다 안전
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdY6biB%2FbtsC302HUEr%2FrNMZTt16S6PBENh8FeYYI1%2Fimg.png" alt="clock" width="350"/>

<br/>

#### 🔗 참고
* [[컴퓨터 공학 기초 강의] 18강. 다양한 보조기억장치(하드 디스크와 플래시 메모리)](https://youtu.be/m2NfFJEvssY?feature=shared)
* [[컴퓨터 공학 기초 강의] 19강. RAID의 정의와 종류](https://youtu.be/lgFmNUG_Atw?feature=shared)
