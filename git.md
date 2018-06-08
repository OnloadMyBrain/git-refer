# Git Reference 



## 영역의 구분

**영역 구분**

- 워킹 트리 working tree (workspace)
- 인덱스 index (planned tree, staging area)
- 로컬 브랜치(현재 브랜치) local branch[local repository]
- 리모트 트래킹 브랜치 remote tracking branch[local repository] : 리모트 브랜치를 로컬에서 참조가능(fetch 명령으로 갱신)
- 리모트 브랜치 remote branch[remote repository]

---

**1. 워킹트리 → 인덱스**

`git status` : 워킹트리에서 아직 인덱스에 add 되지 않은 파일내역

`git diff` : 워킹 트리와 인덱스간 소스내용 차이점

`git diff <파일명>` : 해당 파일에 대한 워킹트리와 인덱스간 소스내용 차이점



**2. 인덱스 → 로컬브랜치**

`git status` : 인덱스에서 아직 commit 되지 않은 파일내역

`git diff --cached` : 인덱스와 로컬브랜치(HEAD) 간의 차이점. "commit"명령을 수행할 경우 반영되는 내용들



**3. 워킹트리 → 로컬브랜치**

`git diff HEAD` : 워킹 트리와 로컬 브랜치(head)의 소스내용 차이. 마지막 commit 이후 변경사항. "commit -a" 명령을 수행할 경우 반영되는 내용들

`git diff HEAD --<파일명>` : 해당 파일에 대한 워킹트리와 마지막 commit 이후의 소스내용 차이점



**4. 워킹트리→리모트 브랜치**

`git diff FETCH_HEAD` : 워킹트리와 리모트 브랜치(FETCH_HEAD)의 소스내용 차이점

`git diff FETCH_HEAD --<파일명>` : 해당 파일에 대한 워킹 트리와 리모트 브랜치 소스내용 차이점



**5. 로컬 브랜치 → 리모트 브랜치 : Outgoing Changes**

`git log FETCH_HEAD..`  : 로컬 브랜치에서 리모트 브랜치에 반영할 변경내역(커밋기록) 

`git diff FETCH_HEAD..`  : 로컬 브랜치에서 리모트 브랜치에 반영할 소스 변경내용 



**6. 리모트 브랜치 → 로컬 브랜치 : Incoming Changes **

`git log ..FETCH_HEAD` : 리모트 브랜치에서 로컬 브랜치로 가져와야할 변경내역(커밋기록) 

`git diff ...FETCH_HEAD` : 리모트 브랜치에서 로컬 브랜치로 가져와야할 소스 변경내용 



*** 참고**

`git fetch` : 기본 리모트 트래킹 저장소의 브랜치들을 업데이트 

`git fetch <리모트트래킹저장소> <리모트브랜치>`  : 해당 리모트트래킹저장소의 리모트브랜치만 업데이트 

`git remote update`  : 트래킹 중인 모든 저장소들 업데이트 

`git pull` : fetch 포함. 즉 FETCH_HEAD 업데이트 됨 

`git log -- <파일명>` : 해당 파일의 커밋 기록 보기 

`git log -p`  : diff 포함해서 보기 

`git log --stat`  : 통계형식으로 보기 

`git log -n` : 최근 n개의 기록만 보기 

`git log <since>..<until>` : 두 커밋 사이의 기록만 보기(둘 중 하나 생략시 HEAD) 

`git diff --ignore-space-change`  :  라인 끝 공백 비교안함 

`git diff --stat`  : 통계형식으로 차이점 보기 

`git diff -- <파일명>` : 해당 파일에 대한 차이점 보기 

`git whatchanged`  : git log와 유사함 

`git show <object>` : 현재 브랜치상의 주어진 커밋에 대한 기본 정보 및 소스변경 내용(diff). obj 생략시 HEAD. log보다 좀더 상세함 

`git reflog`  : 로컬 저장소 내의 브랜치들이 시간에 따라 어떻게 변경되어 왔는지 보여줌 



## 작업의 취소

**개별파일 원복**

`git checkout -- <파일명>` : 워킹트리의 수정된 파일을 index에 있는 것으로 원복 

`git checkout HEAD -- <파일명>` : 워킹트리의 수정된 파일을 HEAD에 있는 것으로 원복(이 경우 --는 생략가능) 

`git checkout FETCH_HEAD -- <파일명>` : 워킹트리의 수정된 파일의 내용을 FETCH_HEAD에 있는 것으로 원복? merge?(이 경우 --는 생략가능) 



**index 추가 취소**

`git reset -- <파일명>` : 해당 파일을 index에 추가한 것을 취소(unstage). 워킹트리의 변경내용은 보존됨. (--mixed 가 default) 

**git reset HEAD <파일명>** : 위와 동일 



**commit 취소**

`git reset HEAD^` : 최종 커밋을 취소. 워킹트리는 보존됨. (커밋은 했으나 push하지 않은 경우 유용) 

`git reset HEAD~2` : 마지막 2개의 커밋을 취소. 워킹트리는 보존됨. 

`git reset --hard HEAD~2` : 마지막 2개의 커밋을 취소. index 및 워킹트리 모두 원복됨. 

`git reset --hard ORIG_HEAD` : 머지한 것을 이미 커밋했을 때, 그 커밋을 취소. (잘못된 머지를 이미 커밋한 경우 유용) 

`git revert HEAD` : HEAD에서 변경한 내역을 취소하는 새로운 커밋 발행(undo commit). (커밋을 이미 push 해버린 경우 유용) 



**워킹트리 전체 원복**

`git reset --hard HEAD` :  워킹트리 전체를 마지막 커밋 상태로 되돌림. 마지막 커밋이후의 워킹트리와 index의 수정사항 모두 사라짐. (변경을 커밋하지 않았다면 유용) 

`git checkout -f` : 변경된 파일들을 HEAD로 모두 원복(아직 커밋하지 않은 워킹트리와 index 의 수정사항 모두 사라짐. 신규추가 파일 제외) 



***참조 : reset 옵션**

`--soft` : index 보존, 워킹트리 보존 (즉 모두 보존)

`--mixed` : index 취소, 워킹트리만 보존 (기본 옵션)

`--hard` : index 취소, 워킹트리 취소 (즉 모두 취소)



***untracked 파일제거**

`git clean -f <파일명>` : 해당파일 제거

`git clean -f -d` : 디렉토리까지 제거

---

공식깃레퍼런스 : [https://git-scm.com/book/ko/v2](https://git-scm.com/book/ko/v2)
