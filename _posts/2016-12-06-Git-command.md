---
layout: post
title: "[Git] Command"
description: Git을 사용해보자
image: 'http://1.bp.blogspot.com/-tvJf5Pp-aSg/VY0R8gIeuZI/AAAAAAAAKSg/CCoLiB_YzuQ/s1600/Softwaredeployment-met-Git-en-SVN-via-SSH.jpg'
category: 'blog'
introduction: Git을 사용해보자
tags:
- git
---

> 명령어는 많이 알 수록 좋다.  
> 다'명'익선

## Basic

### 초기화
해당 디렉토리 접근 후
```
git init
```

## Check

### 현재 상태 확인하기  
```
git status
```  

### commit 이력 보기  
```
git log
```

### commit 이력 한 줄씩 깔끔하게 보기  
```
git log --oneline
```

### commit 이력 그래프로 보기  
```
git log --graph
```

## Stage

### Staged 상태로 올리기  
```
git add <file-name>
```  

### 모든 파일 staged 상태로 올리기
```
git add --all
// or
git add .
```  

### hunk라는 단위로 쪼개서 확인하면서 추가하기  
```
git add -p
```  
이렇게 하면 변경된 코드들을 모두 stage에 올리면서 무심결에 들어간 코드들을 놓치지 않고 하나씩 하나씩 확인할 수 있다.  

## Commit

### commit message 넣어서 하기
```
git commit -m "<commit-message>"
```  

### 이전 X개 commit  지우기
```
git reset --hard HEAD~X
```  

### 바로 이전 commit 수정
```
git commit --amend
```  
(수정 후 :wq 로 저장 후, vi 빠져나오기)    

### 바로 이전 commit에 **파일 추가** 해서 수정하고 싶을 때  
커밋하고 나서 아 '아까 커밋에 수정사항이 반영되어야 하는데?'하는 경우!  
일단 수정완료 후, 해당 파일을 stage에 올리고 이전 커밋을 수정한다.  
```
git add --all  
git commit --amend
```  

### 이미 remote branch에 push를 했다고?  
그렇다면 강제로 다시한번 push해 주면 된다.  
```
git push <remote-name> <remote-branch_name> --force
```  

## Branch

### Local Branch 이름 변경  
```
git branch -m <current-branch-name> <new-branch_name>
```  

### Branch 전체 목록 보기  
```
git branch -a
```  

### Remote Branch 목록 조회
```
git branch -r
```  

### Checkout 하기
```
// 특정 commit으로 checkout
git checkout -b <commit-id(ex. 986a668)>
// 특정 현재 local branch checkout
git checkout -b <local-branch-name>
// 특정 현재 remote branch checkout
git checkout -b <remote-branch-name>
```

### 새로운 branch를 생성하여 Checkout 하기
```
// 특정 commit으로 checkout
git checkout -b <new-local-branch-name> <commit-id(ex. 986a668)>
// 특정 현재 local branch checkout
git checkout -b <new-local-branch-name> <local-branch-name>
// 특정 현재 remote branch checkout
git checkout -b <new-local-branch-name> <remote-branch-name>
```

### Remote branch 갱신하기  
(주로 GitHub 에서 branch 삭제 후 local 목록 조회 시 남아있는 상황)  

```
git remote update
```  

### Remote Branch 생성  
```
git push <remote-name> <new-remote-branch-name>
```  

### Remote Branch 삭제  
```
git push <remote-name> --delete <branch-name>
// or
git push <remote-name> :<branch-name>
```

## Stash

### 현재 상태 Stash 스택에 담기  
커밋하기도 애매하고 잠깐 이 상황을 어따 담아뒀다가 나중에 다시 복구하고 싶을 때!  
스태쉬라는 걸 사용하는데 이름을 정해서 Stash 스택에 담아둔다.  
```
git stash save <stash_name>
```  

### Stash 복구  
다른 작업을 마치고 나서 가장 최근에 담아뒀던 것을 복구하고 싶다면 이것을 사용하면 된다.  
이름이 뭔지 생각 안나면 탭을 누르시오.  
```
git stash apply <stash_name>
```   
이거 말고 간단하게 바로 최근의 것을 가져오는 방법은 아래 방법을 사용해도 된다. 주의할 것은 pop은 가져오고나서 스택에서 사라진다.   
```
git stash pop
```

### Stash 스택 상태 보기  
담아둔 것들이 뭐가 있는지 Stash 스택을 볼 수 있다  
```
git stash list
```  

### Stash 초기화  
stash 스택을 다 날리고 싶다면.  
```
git stash clear
```

### 리모트 저장소 추가하기  
Github에서 특정 Repository를 Fork해서 작업하다가 변경사항을 동기화 해야할 때가 있다.  
리모트 저장소에  해당 Repository 주소를 넣어서 upstream이라는 이름으로 리모트저장소를 추가한다.  
```
git remote add upstream <URL>
```  

### 리모트 저장소 확인  
잘 추가가 되었는지 리모트 저장소를 확인해보자.  
현재 remote Repository 목록을 볼 수 있다.  
```
git remote -v
```  
