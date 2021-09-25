## git branch

[참고 페이지](https://backlog.com/git-tutorial/kr/stepup/stepup1_1.html)

- 독립적으로 어떤 작업을 진행하기 위한 개념
- 기능 단위로 작업의 기록을 남기게 되므로, 문제가 발생하였을 경우 원인이 되는 작업을 찾아거내거나 대책 세우기 쉬워진다



### 통합 브랜치

- 언제든지 배포할 수 있는 버전을 만들 수 있어야 하는 브랜치

  = 이 어플리케이션의 모든 기능이 정상적으로 동작하는 상태

- 일반적으로 저장소를 처음 만들었을 때 생기는 'master' 브랜치를 통합 브랜치로 사용

- 만약 이 어플리케이션에 문제가 생겨서 해당 문제를 수정한다던지, 새로운 기능을 추가한다던지 해야할 때 토픽 브랙치를 만들 수 있다.



### 토픽 브랜치

- 특정 작업이 완료되면, 통합 브랜치에 병합하는 방식으로 진행



### 브랜칭 작업

1. branch는 단순한 포인터(화살표)다.

2. HEAD는 단순한 포인터다. (포인터의 포인터인 경우가 많다.) 

   = 커밋을 직접 가리키는 것보단, 다른 브랜치를 가리키는 경우가 많다. 만약, 커밋을 직접 가리킨다면 경고문구가 나온다.

3. HEAD는 현재 내가 작업중인 커밋을 의미한다.

4. HEAD가 master에 있다 == 현재 master에서 작업중이다.

5. git 에선 `저장` 이 아닌, `commit` 이 작업 완료를 의미한다.



- 브랜치 만들기 `git branch b1`

  - 현재 HEAD가 가리키는 master가 가리키고 있는 커밋에서 브랜치 생성

- 브랜치 옮기기 ` git switch b1` 

  - 현재 HEAD가 가리키는 브랜치를 b1으로 변경

- 브랜치 생성 + 옮기기 `git switch -c b3` 

  - 현재 master가 가리키는 커밋에서 브랜치를 생성하고 해당 브랜치로 이동

- 브랜치 병합 `git merge b1` 

  - 누가 전진할 것인지를 결정 = 어디서 merge 할 것인지
    - 즉,  b2에서 `git merge master` 한 후에도 master 에서 `git merge b2`  가능
    - `$ git log --oneline --graph` 로 그래프로 확인 가능

  1. `Fast-forward` : 빨리 감기 
     - b1이 일궈놓은 길을 master가 따라잡은 것

  2. `Auto Merge` : 자동 병합
     - 필연적으로 새로운 commit (` Merge branch 'b2'`) 을 자동으로 만들어 준다.

  3. 브랜치 병합 충돌 (`CONFLICT`)

  ```python
  Auto-merging secret.txt
  CONFLICT (content): Merge conflict in secret.txt
  Automatic merge failed; fix conflicts and then commit the result.
  
  이후엔 PRO@DESKTOP-MSSB8T3 MINGW64 ~/Desktop/ssafy/git (master|MERGING)
  -> MERGING : 다른 작업보다 병합 먼저 처리해라!
  ```

  - VS code에선 도움 명령어를 보여준다.
    - Accept Current Change 
      - 현재 브랜치(master) 작업 내용 받아들이기
    - Accept Incoming Change
      - 병합하려는 브랜치(b3) 작업 내용 받아들이기
    - Accept Both Change
      - 두 브랜치를 모두 살리기
    - Compare Changes
      - 변화내역 비교하기
  - 수동 작업도 모두 가능 -> 경고문구는 git에서 문자열로 작성한 것이므로 그냥 지우면 된다.

