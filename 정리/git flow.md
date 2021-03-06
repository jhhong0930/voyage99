사실 이전까지는 깃을 제대로 썻다고 할 수 없겠다

협업을 할 때도 모두 default branch에서 같이 작업을 했는데 이번 실전 프로젝트때 올바르게 깃으로 협업을 하는 방법을 조금이라도 터득한 것 같다

### dev branch

우리는 프로젝트 초기에 각각의 기능별 브렌치를 dev에서 뻗어나와 작업 후 다시 dev로 머지시키는 방법으로 작업을 하였다

![image](https://user-images.githubusercontent.com/76515226/141646211-ad93ef30-b6aa-49e2-9a77-ef8d0383126d.png)

흰색이 dev, 나머지가 각각이 만든 기능별 브렌치다, 하지만 이상한 점이 한 가지 있다.

- 만약 A가 작업중이던 기능을 완료하고 dev에 머지 시킨후 B가 작업중이던 기능을 dev에 머지시 A와의 충돌이 있다면?

이전까지는 다행이도 서로의 작업물에 충돌이 없어서 이러한 그림이 나올 수 있었지만 찾아보니 보통은 내 브렌치를 dev에 머지시키기 전에 내 브렌치가 dev에서 뻗어나온 이후로 변경내역이 있다면 dev브렌치를 먼저 내 브렌치에 머지후 다시 dev에 머지를 시키는 것이 일반적이라고 할 수 있겠다

![image](https://user-images.githubusercontent.com/76515226/141646421-a3ce1be0-c23d-4e85-9d19-08d98c860061.png)

여기서는 주황색의 dev에 대한 변경 내역을 보라색이 중간에서 땡겨온 다음 다시 dev에 머지를 시킨다

- 작업 브렌치 생성
- 작업중 dev에 변경사항이 생기면 내 브렌치로 변경 내역을 반영
- 작업 완료 후 dev에 머지
- 다시 브렌치를 생성하여 작업하기 전 로컬 dev 브렌치에서 pull로 원격 브렌치의 변경내역 받아오기

### stash

최근에 프론트와 기능연결이 시작된 이후로 프론트로부터 잦은 버그 수정 요청이 들어왔다

만약 내가 아무런 작업을 하고 있지 않았다면 새 버그 수정용 브렌치를 만들어서 작업을 하면 되겠지만, 만약 내가 무언가 작업을 하고 있었다면 그 내역을 커밋하지 않으면 브렌치를 변경할 수 없는 문제가 생겼었다

그때마다 아직 완료하지 않은 작업을 커밋하는 것도 껄끄러운데 이럴때 유용한 기능을 알게되었다

git stash 명령어를 사용하면 현재 작업중이던 내역을 스택에 저장할 수 있다

- git stash : 작업중이던 내역을 스택에 저장
- git stash list : 저장한 stash 목록 확인
- git stash apply : 가장 최근의 stash를 꺼내와 적용, [stash 이름] 으로 특정 내역을 꺼낼수도 있다
- git stash drop : 가장 최근의 stash를 제거
- git stash pop : stash를 꺼냄과 동시에 제거
- git stash show -p | git apply -R : 가장 최근의 stash를 사용하여 패치를 만들고 그것을 거꾸로 적용

### Pull Request

기존에 작업하던 내역을 dev에 머지시 각자 로컬에서 알아서 해결하였는데, 무조건 pull request를 열고 팀원이 승인을 해주어야 merge를 할 수 있게끔 바꾸었다

- pr을 날리면 모든 팀원은 변경내역을 확인한 후 코멘트와 함께 승인 또는 거절을 한다
- 항상 마지막으로 승인한 사람이 머지시킨다
- 기능별로 pr 단위를 쪼개어 코드리뷰를 함과 동시에 피드백을 할 수 있게끔 개선하였다

![image](https://user-images.githubusercontent.com/76515226/141647069-0765158b-a584-4c53-b653-c79b9fe82b73.png)

깃.. 처음에는 원하는대로 되지도 않아서 짜증만 났는데 점점 알아가는 재미도 있고 뿌듯하다고 할 수 있겠다