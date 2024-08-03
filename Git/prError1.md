## 에러
로컬 컴퓨터에서 깃허브의 원격 리포지토리에 업데이트 내역을 커밋했다. 하지만 GitHub에서는 Compare & pull Request 버튼을 눌렀을 때 아래와 같은 문구가 뜨고 내역을 반영할 수 없었다.

![image](https://github.com/user-attachments/assets/faff2b71-5692-4171-8c2b-91d1ecec561d)


![image](https://github.com/user-attachments/assets/5431614a-a40a-4a7f-9b4f-efc2ed18877e)


<br/>

<br/>

## 발생 이유
기본 브랜치로 잡혀있던 <code>master</code>로 push했기 때문이다. 이렇게 되면 아직 <code>main</code> 브랜치에 미적용된 단계라 저런 문구가 뜨는 것이다.
![image](https://github.com/user-attachments/assets/9ea4648d-67d9-4451-8632-180d94010219)

<br/>

## 해결 방법
Git bash를 이용해 아래 명령어를 순서대로 작성한다.

![image](https://github.com/user-attachments/assets/645b0937-9e2a-41cc-8c48-1ed6f6477798)




> $ git branch main master -f

* git branch [새 브랜치명/이동시킬 브랜치명] [참조할 커밋/브랜치명]
* <code>main</code> 브랜치가 가리키는 커밋을 <code>master</code> 브랜치가 가리키는 커밋으로 '강제로(-f)' 변경

<br/>

> $ git checkout main

* 현재 브랜치를 <code>main</code>으로 변경

<br/>
 
> $ git push origin main -f

* Git 저장소에서 로컬 <code>main</code> 브랜치를 원격 저장소의 <code>main</code> 브랜치로 강제로 push

<br/>


## 결과
commit & push한 내역이 원격 저장소에 잘 반영되었다.

![image](https://github.com/user-attachments/assets/9973db8a-e763-4945-ab04-03f4edc15af5)

<br/>

<br/>

### 🔗 참고
* https://jeongkyun-it.tistory.com/128
* [master를 main으로 바꾸는 문제](https://velog.io/@jswboseok/Git-master%EB%A5%BC-main%EC%9C%BC%EB%A1%9C-%EB%B0%94%EA%BE%B8%EB%8A%94-%EB%AC%B8%EC%A0%9C)
