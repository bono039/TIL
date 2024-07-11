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
   - CPU 내부(레지스터, ALU)에 전달하는 제어 신호
   - CPU 외부(메모리, 입출력장치)에 전달하는 제어 신호

<br/>

## 레지스터
> CPU 내부의 작은 **임시 저장 장치**
- 프로그램 속 명렁어,데이터는 실행 전후로 레지스터에 저장

<br/>

### 반드시 알아야 할 레지스터 ⭐
1. **프로그램 카운터**           : 메모리에서 가져올(=읽어들일) 명령어 주소. 프로그램 실행하면서 1씩 증가
![image](https://github.com/bono039/TIL/assets/67899934/24b98075-9023-4229-9b90-af022829ff88)


2. **메모리 주소 레지스터**   : 메모리 주소 저장 ← _주소 버스_

3. **메모리 버퍼 레지스터**   : 메모리와 주고받을 값 (데이터,명령어) ← _데이터 버스_
![image](https://github.com/bono039/TIL/assets/67899934/875ad74a-1c92-41f6-bd7d-3568b2608278)


4. **명령어 레지스터**           : 해석할 명령어 (방금 메모리에서 읽어들인 명령어)
![image](https://github.com/bono039/TIL/assets/67899934/6599aa44-27d7-4633-9c84-0734b5ffea84)



<br/>

#### 🔗 참고
* [[컴퓨터 공학 기초 강의] 9강. CPU의 내부 구성 - ALU와 제어장치](https://youtu.be/lehWiAsIDrQ?feature=shared)
* [[컴퓨터 공학 기초 강의] 10강. CPU의 내부 구성 - 레지스터](https://youtu.be/fSCHizcezTs?feature=shared)
* [[컴퓨터 공학 기초 강의] 11강. 명령어 사이클과 인터럽트](https://youtu.be/3Yz7OnVUM28?feature=shared)
