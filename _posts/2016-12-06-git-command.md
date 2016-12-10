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

### commit message 넣어서 하기
`git commit -m "<commit-message>"`

### 이전 X개 commit  지우기
`git reset --hard HEAD~X`

### 직전 commit 메시지 수정
`git commit --amend`
(수정 후 :wq 로 vi 빠져나오기)

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
