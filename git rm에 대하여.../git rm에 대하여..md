`git rm`에 대하여 이해하기위해서 [여기](https://dololak.tistory.com/310)에서 공부를 했었는데 잘 이해가 되지않았었다. 근데 최근들어서 다시 보게 되었는데 너무 좋은 글이였다. 이제 시작해보자.

우선 들어가기에 앞서 `git-scm`은 `git rm`을 어떻게 소개하고있는지 봅시다.

> git-rm - Remove files from the working tree and from the index

즉, 작업 디렉토리와 index에서 파일을 지운다는 것입니다. git-scm에서는 staging영역을 공식적으로 index라고 칭하고 있습니다.

즉 파일도 실제로 지워주고 staging영역에 있다면 staging영역에서 삭제 해준다는 뜻이겠죠 ?

# Git에서 파일 삭제하는법

Git에서 파일을 삭제하고자 할때는 `git rm`을 통해 `Tracked` 상태의 파일을 제거하여 `Untracked`상태로 만들고 제거했다는 내역 자체를 커밋해야 합니다.

[##_Image|kage@mt7MB/btq6htoyS5P/Z42OnJx8B82GXLCmHxj0wK/img.png|alignCenter|width="100%" data-origin-width="958" data-origin-height="400" data-ke-mobilestyle="widthOrigin"|||_##]

  
위의 사진은 `git rm`명령어를 실행한 모습인데 `git rm`을 실행하기 이전 `ls`로 현재 `one`, `two`, `three` 총 3개의 파일이 있는것을 알수 있습니다. 그후 `git rm`명령어를 통해 `one`이라는 파일을 삭제했고 `git status`명령어로 변경사항을 확인해보니 `Changes to be committed`의 `deleted`된 `one`파일이 스테이징 영역에 올라가 있는것을 확인할 수 있는데요, 이것은 그냥 저희가 파일을 평소처럼 수정하고 `git add`를 해준것과 다를게 하나도 없습니다.

[##_Image|kage@lCMOo/btq6gM2Xqce/R45fKFLPXhpezpkBY3zHOk/img.png|alignCenter|width="100%" data-origin-width="522" data-origin-height="461" data-ke-mobilestyle="widthOrigin"|||_##]

위의 사진에서 저는 three라는 파일에 변경사항을 주고 상태를 확인했는데 `Changes not staged for commit`상태의 `three`파일을 볼수 있습니다. 이것을 `git add`하자 방금 `git rm`으로 삭제한 `one`파일과 동일한 스테이징 영역에 있는것을 확인할 수 있습니다. 이것이 제가 `git rm`도 결국 다른파일과 다를게 없다고 하는 이유입니다.

# 사실 one파일은 삭제됬지만 삭제되지 않았다.

이게 무슨말일까요 ? 분명 `ls`명령어로도 삭제된것이 확인되었고 `git status`를 통해 상태를 확인해 보아도 `deleted`상태인데 왜 제가 삭제됬지만 삭제되지 않았다고 할까요 ? 깃은 스테이징 영역에 변경사항을 저장한다고 해도 정말 그 변경사항이 적용된 것이 아닙니다. 아직은 아니지만 커밋을 하게되면 그제서야 비로소 변경사항들이 실제 적용 및 저장이 됩니다. 제가 한번 `one`파일을 다시 살려보겠습니다

아까 stage영역에 있던 두개의 파일이 분명히 있는데 `git reset --hard` 명령어를 통해 `one`파일과 `three`파일이 마치 삭제되지도 않고, 변경되지 않은것처럼 만들게 되었습니다 실제로 다시 `ls`명령어를 쳤을때 `one`이 살아있는것을 볼수있습니다 ! 즉, `git reset --hard`명령어를 입력하기 이전 상태에서 `git commit`을 해야 비로소 `one`파일이 삭제되고 `three`파일이 내용이 변경되는겁니다. 그럼 `one`파일은 삭제된 파일이므로 더이상 추적하지도 않고 추적을 할수도 없겠죠 ? `three`는 삭제되진 않았으니 추적은 여전히 가능합니다.

# 그럼 git rm말고 그냥 rm으로 삭제하면 ?

`git rm`과 같이 git의 명령어가 아닌 운영체제 단의 삭제 명령어인 `rm`으로 삭제하면 어떻게 될까요? 당연히 잘 삭제되겠죠 ? 그럼 깃에서는 이 삭제된 파일을 어떤상태로 우리에게 표현해줄지도 알아봅시다.

[##_Image|kage@eha9hC/btq6fUf8qfC/vB3KaNfQAI8RRPrSBapNI0/img.png|alignCenter|width="100%" data-origin-width="518" data-origin-height="459" data-ke-mobilestyle="widthOrigin"|||_##]

`git rm`이 아닌 `rm`으로 `one`을 삭제하고 상태를 봐보니 스테이징되지 않은 deleted상태의 one파일이 있습니다. 이것을 `git add`해주니 `git rm`을 했을때랑 똑같은 결과가 나옵니다 ! 그럼 여기서 유추할수 있는 결론은 ?

`git rm` = `rm` + `git add` 이라는거죠 즉 `git rm`은 rm을 해주고 git add를 또 해주는 우리의 귀찮음을 덜어줄수있는 아주 착한 명령어라는 것입니다.

[##_Image|kage@bJZLGq/btq6herw2P2/tRqvGBYGwa97CEgmGYw871/img.png|alignCenter|width="100%" data-origin-width="530" data-origin-height="563" data-ke-mobilestyle="widthOrigin"|||_##]

자 이번엔 위 사진에서 two라는 파일을 삭제해볼껀데 `rm`후 `git rm`을 해보겠습니다.결과가 똑같죠 ? 지금 계속 똑같은 얘기하고 있는겁니다!

# 수정한 파일 또는 Staged 상태인 파일을 삭제하려고하면?

만약 워킹디렉토리에서 파일을 수정한후 커밋도 안했는데 파일을 삭제해버리려고 하면 git은 오류를 냅니다. 이때는 `-f`옵션을 통해 강제 삭제를 진행해주시면 되는데요 아래 사진은 **이미 stage 영역에 올라가 있는 파일을 삭제하려고 했을때의** 오류 내용입니다.

[##_Image|kage@blJ3bq/btq6gcOmE3i/y6LEAGgBzzlPcZrhSjgKm1/img.png|alignCenter|width="100%" data-origin-width="533" data-origin-height="685" data-ke-mobilestyle="widthOrigin"|||_##]

이미 스테이지 영역에 올라가 있다는 말은 변경사항을 커밋을 하려고 저희가 준비를 시켜놨다고 깃에게 알려주는것이여서 깃은 아 이게 저장(commit)을 해야하는 중요한 변경사항이구나 라고 인식을 하고있습니다. 근데 갑자기 그 중요한것을 삭제하려한다? 용납못하겠죠? 그래서 오류를 띄우는 것입니다. 오류 내용도 three라는 파일이 staging영역에 있다는 내용이죠. 근데 그래도 사람이 우선이니 강제로 삭제하고 싶다면 앞서 말씀드린것처럼 `git rm -f`를 통해서 강제삭제가 가능합니다. 자 다음 두번째 경우를 보시죠 이 경우는 **변경사항이 생겼는데 삭제하려고 하는것**입니다.

[##_Image|kage@oxpxp/btq6gM9JOuk/ejBejM71aMv1646QY4hLn1/img.png|alignCenter|width="100%" data-origin-width="513" data-origin-height="478" data-ke-mobilestyle="widthOrigin"|||_##]

보시다시피 수정사항이 생겼으니 에러를 깃이 냈습니다. 이 두가지의 경우에서 역시 깃은 버전관리툴이라는 이름에 맞게 변경사항에 아주 민감한 친구입니다.  
허용을 해주는게 없어요 그럼 저희는 강제삭제하면되겠죠? 하지만 정말 특별한 경우아니면 -f옵션은 어디서나 비추입니다. 혹시 잘못되면 큰일나잖아요 ㅎㅎㅎ

## Git 저장소에만 삭제하고 실제 파일은 남겨두고 싶은 경우

만약에 실제 로컬에는 파일을 남기고 Git 저장소에서만 삭제하고 싶은 경우에는 `--cached`옵션을 사용하면 됩니다. 자 집중해서 보시죠! 이번엔 조금 특이하니까 집중해서 봐야합니다 !

[##_Image|kage@cDDUny/btq6fTar1WI/rtFMCeJndqqskLPRQzdKA0/img.png|alignCenter|width="100%" data-origin-width="564" data-origin-height="952" data-ke-mobilestyle="widthOrigin"|||_##]

이 경우는 로그랑 같이 보면 좋을거 같아서 로그까지 준비했습니다. 아까처럼 똑같이 `one`, `two`, `three` 파일을 만들어서 커밋을 한번 해줬습니다.  
로그 한개 찍히는거 보이시죠 ?

그리고 이번엔 그냥 `git rm`이 아닌 `--cached`옵션까지 추가해서 삭제 해보았습니다. 그러고나서 상태를봤더니 ? `deleted`된 `one`파일과 Untracked file인 `one`이 보입니다. 이 말은 `one`파일이 추적대상에서만 제외되고 실제로는 존재한다는 뜻입니다. cached라는 이름의 옵션이 붙은 이유는 유추하건데 보통 IT에서 캐싱이라는것은 저장하는것을 말합니다. 메모리에서 자주사용하는 데이터를 캐시에 저장해놓는것을 말하는데요, 실제로 웹사이트를 개발하다보면 크롬은 자주 방문하는 사이트를 캐싱해놓고 사용자의 요청이 있을때마다 캐싱된 데이터를 화면에 뿌려줍니다 이게 성능에는 좋지만 웹사이트 개발자에겐 치명적일수 있는데요 왜냐하면 아무리 새로고침을 해도 내가 변경한 CSS가 안먹기 때문입니다. 그전과 똑같은 화면을 계속뿌려주니 개발자는 오류도 안나는데 왜 도대체 아무런 반응이없는건지 헷갈리게 되죠 [여기](https://godtaehee.tistory.com/1?category=970301) 제가 실제 겪었던 경험담을 읽어보시면 제 말이 이해가 더 잘되실겁니다. 제가 이거때문에 정말 날린 시간이 몇시간인지 .. ㅋㅋㅋ 무튼 그런 뜻에서 --cached라는 옵션의 이름이 나오지 않았나 싶습니다. 추적대상에서만 제외시킬 뿐이지 실제로는 파일이 존재하거든요 그럼 이걸 어디에다 써먹을까요 ?

바로 로컬에는 존재하게 하고싶고, Github(원격)에는 존재하지 않게 하고싶을때 사용합니다. 자 말보단 그냥 실습이 훨씬 낫습니다.

[##_Image|kage@cEwrPX/btq6gMomD6R/waxsdCEtw5odkVJjk9Vt3k/img.png|alignCenter|width="100%" data-origin-width="952" data-origin-height="482" data-ke-mobilestyle="widthOrigin"|||_##]

위의 사진처럼 `git rm --cached` 테스트를 위한 빈 레포를 깃헙에 만듭니다.

[##_Image|kage@bwIPpM/btq6erkW2yC/cwKxJZoMgqRgZ6SbTKDyV1/img.png|alignCenter|width="100%" data-origin-width="434" data-origin-height="532" data-ke-mobilestyle="widthOrigin"|||_##]

그다음 우선 push로 저 빈레포에 올립니다.

[##_Image|kage@lEcfC/btq59NhNQ54/vsEfSE9LHhZ8kKFHpPt7n0/img.png|alignCenter|width="100%" data-origin-width="948" data-origin-height="519" data-ke-mobilestyle="widthOrigin"|||_##]

올라갔죠?

[##_Image|kage@KwQcr/btq59OHKK6L/EZCouwJSuSld27KXlQqOr1/img.png|alignCenter|width="100%" data-origin-width="478" data-origin-height="648" data-ke-mobilestyle="widthOrigin"|||_##]

자 이제 중요합니다. 저는 `git rm --cached`로 로컬에는 존재하지만 추적대상에서 제외를 시켰습니다. 그렇게 하고난후 커밋후 푸시까지 마쳤는데요 깃헙에 가서 결과가 어떻게 되어있는지 봅시다. 보기전에 ls 명령어로 제 로컬에 분명히 one, two, three 파일이 있음에 주목합니다. 그리고 커밋 해쉬값을 보면 `ae4fe06..1bf300b`이렇게 되어있는데 이것도 유의깊게 보세요

[##_Image|kage@xZaxI/btq57Gb78yK/6vR4NHDZeyKGt14CkerK31/img.png|alignCenter|width="100%" data-origin-width="713" data-origin-height="387" data-ke-mobilestyle="widthOrigin"|||_##]

one 파일이 깃헙에는 존재하지 않는걸 확인할수 있죠 ?

[##_Image|kage@yFFOp/btq6gA9uuX4/cizKkAIKnnLSWUCUmXIMe1/img.png|alignCenter|width="100%" data-origin-width="713" data-origin-height="387" data-ke-mobilestyle="widthOrigin"|||_##]

그리고 커밋들의 해쉬값들은 각각 `ae4fe06`,`1bf300b`임을 확인할수 있고

[##_Image|kage@bg5j6R/btq6erL2RYf/hw2RYwlhIWHeAV3KDxGb3K/img.png|alignCenter|width="100%" data-origin-width="583" data-origin-height="157" data-ke-mobilestyle="widthOrigin"|||_##]

로컬에도 저 커밋들이 실제 존재하는것을 볼수있습니다. 깃헙에서도 현재 커밋은 1bf300b이고 로컬에서도 똑같이 1bf300b임을 확인할 수있습니다. 이렇게 보통 내 로컬에는 존재하지만 원격에서는 삭제하고싶은경우에 --cached옵션을 사용합니다. 더 자세히 예를들어보면 설정파일 같은것들, 그런것들을 원격에서 제거해주지 않으면 사람마다 컴퓨터 환경이 다른데 내 깃헙 레포를 가지고 협업을 하려는 사람이 만약 clone혹은 pull을 받을때 그 사람의 설정파일이 완전히 깨져버리는 현상이 나타납니다. 이건 정말정말정말정말 치명적입니다.. 다 경험해봤기때문에 이렇게 제가 말씀을... 이건 저한테뿐만 피해가 아니라 추후에 다른분들한테도 피해가 갑니다. 그럴때 이것을 미리 방지해주기 위해 --cached 옵션을 사용하면 유용할 것입니다.
