# git merge VS git rebase 란?
Git 에서 한 브랜치에서 다른 브랜치로 합치는 방법은 `git merge` 와 `git rebase`명령어가 있습니다.
`git merge`와 `git rebase`는 둘 다 합치는 관점에서는 같지만 **commit history가 달라집니다.**

<br><br><br>

## git merge
쉽고 안전하지만 커밋히스토리가 지저분할 수 있습니다.
두 브랜치의 최종결과만을 가지고 합칩니다.

<br><br><br>

## git rebase
잘 모르고 사용할 경우 위험할 수 있어 까다롭지만 커밋히스토리를 깔끔하게 관리할 수 있습니다.
브랜치의 변경사항을 순서대로 다른 브랜치에 적용하면서 합칩니다.
`git rebase`한 브랜치의 Log 를 살펴보면 히스토리가 선형입니다. 일을 병렬로 동시에 진행해도 `git rebase`하고 나면 모든 작업이 차례대로 수행된 것처럼 보입니다.
`git rebase`를 하면서 동시에 `squash` 를 사용해 커밋을 하나로 정리 할 수 있습니다.
따라서 `git rebase`는 보통 리모트 브랜치에 **커밋을 깔끔하게 적용하고 싶을 때 사용**합니다.

`git rebase`는 base를 새롭게 설정한다는 의미로 이해하면 좋습니다. → `git rebase [newbase]`
메인 프로젝트에 Patch 를 보낼 준비가 되면 하는 것이 `git rebase`니까 브랜치에서 하던 일을 완전히 마치고 **origin/master**로 `git rebase`합니다. 이렇게 `git rebase`하고 나면 프로젝트 관리자는 어떠한 통합작업도 필요 없습니다. 그냥 `master`브랜치를 **Fast-forward**시키면 됩니다.

<br><br><br>

## git rebase 명령어
> `git rebase -i [수정을 시작할 커밋의 이전 커밋]`<br>

`-i` 는 `--interactive` 의 약어로 말그대로 `git rebase` 명령어를 대화형으로 실행하겠다는 의미입니다.
이렇게 명령어를 실행하면 `"수정을 시작할 커밋의 이전 커밋" ~ "현재 커밋(HEAD)"` 범위에 있는 모든 커밋들의 리스트가 출력된다. 예를 들어  `git rebase -i HEAD3`를 실행하면 HEAD2, HEAD1, HEAD 커밋들이 출력됩니다.<br>

> git reset --hard HEAD^ // commit 되돌리기<br>
> git commit --amend // 이전 커밋 메시지만 변경할 때

잘못 리베이스를 했다면
> git rebase —-abort // `rebase` 도중  
> // rebase 완료 후  
> git reflog // 돌아갈 지점 찾기  
> git reset --hard 돌아갈지점 // 복구

충돌 발생 시 에디터에서 충돌 해결 후
> git add .
> git rebase -—continue

반복하되, commit을 해줄 필요는 없습니다.

<br><br><br>

## git rebase -i [수정을 시작할 커밋의 이전 커밋] 명령어를 실행하면 열리는 에디터 창 용어

### pick
**pick** 은 커밋을 사용하겠다는 의미입니다. 이를 이용해서 커밋의 순서를 바꿀 수도 있고, 커밋의 해쉬값을 이용해 특정 커밋을 가져올 수도 있습니다.
기본적으로 pick으로 설정되어있기 때문에 아무것도 변경하지 않고 종료한다면, 커밋에 대해 어떤 변경도 일어나지 않습니다.

### reword
**reword**는 커밋 메시지를 변경하는 명령어입니다. 커밋 메시지를 변경할 커밋 앞에 reword 명령어를 쓴 후 저장하면, 해당 커밋의 메시지를 다시 작성하는 에디터 창이 열립니다. `-i`가 **대화형**명령어 옵션이라고 했던 의미가 이제서야 체감될 것입니다.

### edit
앞서 설명한 **reword**는 커밋 메시지만 변경하는 명령어였다면, **edit** 은 커밋 메시지 뿐만 아니라 커밋의 작업 내용도 변경할 수 있는 명령어입니다.

### squash
**squash** 은 해당 커밋을 이전 커밋과 합치는 명령어입니다.

<br><br><br>

# 참고
`rebase` 도중 충돌이 일어날 경우 마치 코드가 날아간 것 처럼 보일 수 있습니다. 코드가 사라진 것이 아니라 코드를 합치던 도중 중단된 것입니다. 충돌을 해결하고 남은 과정을 끝까지 진행하면 모든 코드가 다 들어와 있게 됩니다.

<br><br><br>

# 주의사항
`git rebase` 는 이전의 커밋 히스토리를 변경하기 때문에 항상 주의해서 사용하여야 합니다. 만약 이미 GitHub과 같은 원격 저장소에 push까지 한 커밋이라면 변경한 커밋들은 원격 저장소에 push되지 않을 것 입니다.이 때 `git push --force` 또는 git `push -f` 명령어로 강제로 원격 저장소에 커밋 히스토리를 덮어쓸 수도 있습니다. 하지만 만약 이전에 push한 커밋들을 다른 개발자들과 공유하고 있었다면 커밋 히스토리의 불일치가 발생해 흔히 말하는 "git이 꼬였다." 하는 상황이 발생할 수 있습니다. 따라서 협업 시 브랜치를 공유하는 상황에서 `$ git rebase` 는 가급적이면 지양하여야 합니다.

<br><br><br>

# 결론
`git rebase`를 하든지, `git merge`를 하든지 최종 결과물은 같고 커밋 히스토리만 다르다는 것이 중요합니다.

<br>

> 출처 : [https://velog.io/@remon/Git-Workflow-Rebase](https://velog.io/@remon/Git-Workflow-Rebase)
