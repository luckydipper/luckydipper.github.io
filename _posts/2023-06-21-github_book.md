---
layout: post
title: "팀 개발을 위한 git github 시작하기"
categories:
  - Book review
tags:

last_modified_at: 2023-06-21
comments: true
---
![archtecture](/assets/img/Book review/git_book_cover.jpg)

## 1. github이란? 
git의 버전 관리 시스템을 다른 사람과 협업하여 쓸 수 있는 오픈소스 프로그램입니다.  
단 학교에서 가르쳐 주지 않습니다. 스스로 공부합니다. 대학교 1학년 git github을 처음 배우며 맨붕 했던 기억이 있습니다.  
github은 오픈 소스를 가져올 수 있다는 것 때문에 반드시 알아야 합니다. 
예를 들어, 저번에 새로 산 노트북에 우분투 22.04를 듀얼부팅 했을 때 문제가 생겼습니다. 우분투에서 와이파이 드라이버 Realtek RTL8852BE가 인식 되지 않는 것입니다.  
구글링 해서 https://github.com/HRex39/rtl8852be에서 가져와 직접 컴파일 해 쓸 수 있었습니다.  

## 2. 책의 내용 
읽은 내용 중 기억에 남는 내용 위주로 적어 봤습니다.

- checkout은 두 가지 사용처가 있습니다.  
1. change branch
2. recover work tree file content
두 기능은 따로 다른 명령어로 분해 됐습니다.  
switch : 브랜치 변경  
restore : 파일을 복구 하는 명령   
이때 체크섬으로 checkout 하면, detached head가 되므로, 좋지 않습니다. 위의 두 명령어를 더 자주 사용합니다.  

- main master는 git에서 프로젝트를 만들었는지 github에서 만들었는지에 따라 다릅니다.  

- 내 branch에서 main branch로 바로 merge 하지 말고, main을 내 branche에 marge 해본 후 에러가 없으면 main에 merge 하는 것이 좋습니다.  

- main branch에 tag를 붙혀서 버전을 나타낼 수 있습니다.  
- PR 보내기 위해서 fork한 후, upstream 등록한 후 rebase해서 fetch한 repo에 강제로 push 해야 합니다.  
- amend : push 이전 commit 수정합니다. 만약 push 했다면, 강제로 push 해야 합니다.  
- cherry-pick : 중간의 하나의 commit만 가져와서 반영하기  
- reset : soft mixed hard로 나뉘며 commit된 것을 원래 상태로 되돌리거나 없앱니다. mixed와 soft의 차이는 이전 것을 stage에 둘 것인가 말 것인가의 차이입니다. 원격 저장소에도 반영 도리려면 강제 push 해야합니다. 198p  
- revert : 새로운 커밋을 추가해 커밋을 되돌리는 방법입니다. 협업 할 때, 충돌 나지 않게 하기 위해 새로운 commit을 만드는 것입니다.  
- stash : 잠시 다른 commit으로 옮겨 갈 때, 임시로 저장하는 법.  
- PR 이름 짓는법
- 브랜치 보호 하는 법.  
- commit hash, commit checksum : commit 후 log에 나오는 문자열.  
```
git log -n3 : show 3 latest commit
git log --online --graph --all --decorate
```

## 3. 책의 장점
1. 자세합니다. 책이라서 체계적으로 정리가 잘 돼어 있습니다.  
생활코딩, 얄코 등등 많은 깃헙 강의가 있었는데 강의 형식이다 보니, 정리하기가 힘들었습니다.  
2. 그림을 통해 이해하기 쉽습니다. 동영상 강의 처럼  그림이 많습니다.  
3. GUI sourse code 다루는 법을 알려준다.  
처음 깃을 배울 때, CLI로 하기는 어려웠습니다. source code GUI로 이해 한 후, CLI로 가는 것이 좋다고 생각합니다. 
저도 처음 할 때 소스코드로 GUI를 보면서 코딩했었습니다.  

## 4. 책의 단점 
1. 완전 초보가 보기엔 너무 자세합니다.  
책을 한개 한개 뜯어서 읽고 있기 보다는 직접 처보는 것이 좋을 것 같습니다.  
2. 친구랑 하면 더 좋을 것 같습니다.  
솔직히 github id 2개 만들어서 PR 하는 것까지 하기는 힘들것 같습니다.  
친구와 스터디를 만들어 같이 협업해 보는 것을 더욱 추천합니다.  

"한빛미디어 \<나는 리뷰어다\> 활동을 위해서 책을 제공받아 작성된 서평입니다."