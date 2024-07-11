# CPU의 작동 원리
> ALU와 제어장치
> 
> 레지스터
>
> 명령어 사이클과 인터럽트

<br/>

## ALU와 제어장치
![image](https://github.com/bono039/TIL/assets/67899934/feed4098-2a8e-42eb-92f1-df24228f8199)

<br/>

### 1. ALU : 계산 장치

![image](https://github.com/bono039/TIL/assets/67899934/b2755cfd-d76e-4817-8369-e3968685e25e)

- **받아들이는 정보** | 피연산자, 제어신호
- **내보내는 정보**     | 결괏값(숫자, 문자, 주소 등), 플래그(연산 결과에 대한 부가 정보)
<details>
<summary>플래그 레지스터</summary>
<div markdown="1">

![image](https://github.com/bono039/TIL/assets/67899934/bdee186d-9cde-46ed-9c45-a682f7e85824)
![image](https://github.com/bono039/TIL/assets/67899934/d827e626-a0fa-454a-b316-7b3cf0b8d0b3)


</div>
</details>


<br/>

### 2. 제어 장치 : 제어 신호 발생시키고 명령어 해석하는 장치
![image](https://github.com/bono039/TIL/assets/67899934/e1fde85f-2b07-46c2-a62b-43a0c64fea87)


- 받아들이는 정보
   1. <code>클럭</code> : 컴퓨터의 모든 부품을 일사불란하게 움직이게 하는 시간 단위
   2. 해석할 명렁어
   3. 플래그
   4. 제어 신호
- 내보내는 정보
   - CPU 내부에 전달하는 제어 신호 (to 레지스터, ALU)
   - CPU 외부에 전달하는 제어 신호 (to 메모리, 입출력장치)



<br/>

#### 🔗 참고
* [[컴퓨터 공학 기초 강의] 9강. CPU의 내부 구성 - ALU와 제어장치](https://youtu.be/lehWiAsIDrQ?feature=shared)
* [[컴퓨터 공학 기초 강의] 10강. CPU의 내부 구성 - 레지스터](https://youtu.be/fSCHizcezTs?feature=shared)
* [[컴퓨터 공학 기초 강의] 11강. 명령어 사이클과 인터럽트](https://youtu.be/3Yz7OnVUM28?feature=shared)
