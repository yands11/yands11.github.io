---
layout: post
title: Git - Command 정리
---

# Git Command 정리

---

얼마 되지는 않았지만 git으로 프로젝트 관리를 하는 회사로 옮기게 되었습니다.  
그전에는 git을 알고 사용(?)하고 있긴했지만, 제대로 아는게 아니었고 커맨드라인에서 쓰는 명령어는 많이 몰랐었어요.  
그래서 이참에 자꾸 까먹는 command들을 list-up하고 꾸준히 추가해 보도록 하겠습니다. 👍    

### 현재 상태 확인하기  
`git status`  

### Staged 상태로 올리기  
`git add <file-name>`  

### 모든 파일 staged 상태로 올리기
`git add --all`  

### hunk라는 단위로 쪼개서 확인하면서 추가하기  
`git add -p`  
이렇게 하면 add --all 하면서 무심결에 들어간 실수한 코드들을 하나씩 하나씩 확인할 수 있다.  
하지만 꽤나 귀찮다...ㅎㅎ  

### commit message 넣어서 하기
`git commit -m "<commit-message>"`  

### 이전 X개 commit  지우기
`git reset --hard HEAD~X`  

### 바로 이전 commit **메시지만** 수정
`git commit --amend`  
(수정 후 :wq 로 vi 빠져나오기)    

### 바로 이전 commit에 **파일 추가** 해서 수정하고 싶을 때  
커밋하고 나서 아 '아까 커밋에 이 파일들 더 들어가야 하는데?'  
'아까 커밋에 이 내용 수정되어야 하는데?' 하는 경우!!!!  
일단 수정완료 후, 해당 파일을 stage에 올린다.  
`git add --all`  
그리고 나서 이전 커밋을 수정한다.  
`git commit --amend`  
(위와 같이 메시지도 수정하고 싶으면 수정하고 :wq 로 vi 빠져나오기)  
자! 이제 로컬에는 커밋이 수정되었다.  
이미 remote branch에 push를 했다고?  
그렇다면 강제로 다시한번 push해 주면 된다.  
`git push <remote-name> <remote-branch_name> --force`  

### Local Branch 이름 변경  
`git branch -m <current-branch-name> <new-branch_name>`  

### Branch 전체 목록 보기  
`git branch -a`  

### Remote Branch 목록 보기  
`git branch -r`  

### Remote Branch 삭제(변동) 후, Local에서 Remote branch 목록 갱신하기  
(주로 GitHub 에서 branch 삭제 후 local 목록 조회 시 남아있는 상황)  

`git remote update --prune`  

### Remote Branch 생성  
`git push <remote-name> <new-remote-branch-name>`  

### Remote Branch 삭제  
`git push <remote-name> --delete <branch-name>`  

`git push <remote-name> :<branch-name>`

### Remote Branch를 Local Branch로 가져오기  
`git checkout -b <new-local-branch-name> <remote-branch-name>`

### 현재 상태 Stash 스택에 담기  
커밋하기도 애매하고 잠깐 이 상황을 어따 담아뒀다가 나중에 다시 복구하고 싶을 때!  
스태쉬라는 걸 사용하는데 이름을 정해서 Stash 스택에 담아둔다.  
`git stash save <stash_name>`  

### Stash 복구  
다른 작업을 마치고 나서 가장 최근에 담아뒀던 것을 복구하고 싶다면 이것을 사용하면 된다.  
`git stash apply <stash_name>`    
이거 말고 간단하게 하나만 담아뒀다면 아래 방법을 사용해도 된다. 주의할 것은 pop은 가져오고나서 스택에서 사라진다.   
`git stash pop`  

### Stash 스택 상태 보기  
담아둔 것들이 뭐가 있는지 Stash 스택을 볼 수 있다  
`git stash list`  

### Stash 초기화  
stash 스택을 다 날리고 싶다면.  
`git stash clear`
