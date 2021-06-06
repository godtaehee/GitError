# 들어가기 앞서..

`stash`는 꼭 자기가 `stash`했던 내역을 `stash`했던 시점에 다시 적용시킬 필요는 없습니다. 한번 stash한 내용은 어느 커밋, 어느 브랜치에도 적용가능합니다 그 와중에 충돌이 생긴다면 해결을 해주면 그만입니다. 저는 `stash`를 하고나면 꼭 다시 `stash`했던 시점으로 돌아와 `stash`스택에 저장된 내역을 적용시켜야 하는줄 알았습니다 밑에서 들 예는 같은 브랜치에 다시 돌아와서 적용시키는 예이지만 꼭 저렇지 않고 다른 브랜치, 다른 커밋에 `stash`를 적용시켜도 무방합니다.

[##_Image|kage@xl05Z/btq6k8Mv4Gj/tE4K8xlyibEHCe1TYtckQK/img.png|alignCenter|width="100%" data-origin-width="566" data-origin-height="355" data-ke-mobilestyle="widthOrigin"|||_##]

현재 HEAD가 가리키는 main 브랜치에서 갑자기 develop브랜치에 볼일이 있어서 develop 브랜치로 checkout 하려는 상황이라고 가정 해보겠습니다. 예를들어 develop브랜치에는 내가 main브랜치에서 수정하려는 파일의 상태가 어땠는지를 확인하기위해서 잠시 develop브랜치로 checkout하고 금방 main브랜치에 다시 돌아오려고 합니다.

[##_Image|kage@cIMoff/btq6n5A2Fj2/feZQ934Wi54vxYja7o1MD1/img.png|alignCenter|width="100%" data-origin-width="574" data-origin-height="346" data-ke-mobilestyle="widthOrigin"|||_##]

그런데 저는 `one`이라는 파일을 수정중이였고 `stash`의 성질을 전혀 모르는 저는 바로 develop 브랜치로 `checkout`을 하려고 하는데 `error`가 발견됩니다 에러의 내용은 지금 `one`이라는 파일에 변경사항이 생겨서 `checkout`을 하게되면 그 파일이 그냥 덮혀 씌워져버릴것이니 `chekcout`하기 전에 커밋하거나 `stash`하라는 내용의 에러를 발생시킵니다. 왜 이러한 에러를 발생시킬까요 ?

## 착한 깃

비록 우리에게 오류를 내보내 주지만 이것은 우리의 소중한 파일을 깃이 덮혀씌워지게 하지 않게 하기위해 애초에 `checkout`을 막고 있는 것입니다. `main`브랜치의 `one`이라는 파일에 어떤 작업을 거의 마친상태이며 `develop`브랜치에는 `one`이라는 파일을 이제 막 만들었다고 생각하면 당연히 `develop`브랜치로 `checkout`하면 안됩니다. 왜냐하면 만약 `checkout`을 허용한다면 `one`이라는 파일을 작업 다해놨는데 완전 초기상태로 돌려보내기 때문입니다.

## 그럼 만약 main 브랜치에는 있지만 develop브랜치에는 없는 파일이 변경사항이 생기면 checkout이 가능할까 ?

지금의 궁금증을 해결하기위해 폴더를 새로 파서 환경을 다시 만들었다 상황은 main브랜치와 develop가 있는상황과 똑같다.

무슨말이냐면 앞서 상황에서는 main브랜치와 develop브랜치 둘다 가지고 있는 `one`이라는 파일의 변경사항이 덮혀씌워질까봐 checkout을 안시켜준것이였다 그럼 다음 사진을 먼저 보자

[##_Image|kage@VNGEf/btq6n6fFwcv/elpeGovaKTfmX0i2YQl7nk/img.png|alignCenter|width="100%" data-origin-width="762" data-origin-height="766" data-ke-mobilestyle="widthOrigin"|||_##]

위의 상황은 develop브랜치에 현재 위치해 있고 파일의 목록이 `one`, `two`, `three`로 3개가 있는 것을 확인할수 있으며 `main`브랜치에는 `four`까지 총 4개가 있는것을 확인할수 있는데 이때 내가 `four`파일에 변경사항을 준 상태로 develop브랜치로 `checkout`하려고 했다. 내가 왜 이런 상황을 만든지 이해가 됬나 ? 지금 일부로 `main`브랜치에는 있고 develop브랜치에는 없는 파일을 수정한것이다. 둘이 겹치는 파일이 아니니 덮혀 씌울게 없으니까 `checkout`이 될것이다라고 궁금증이 생겼기때문이다 결론은 똑같이 error를 발생시킨다. 이는 `git checkout`은 있는 파일이든 없는 파일이든 `Tracked`상태인 파일을 수정하거나 `Staged Area`에 파일이 있다면 `git checkout`을 허용하지 않기 때문이다. 사실 우리 눈에야 저 파일이 `four`라는 영어로 보이는것이지 사실 컴퓨터 입장에서 보면 `four`라는 파일은 그냥 이진수로 이름 지어진 파일일 뿐이다. 컴퓨터는 0과 1밖에 모르기 때문이다. 그렇기 때문에 `git checkout`의 기준이 이름이 같은게 있는가? 가 아닌 위에서 말한 Tracked 상태의 파일을 수정하거나 Staging Area에 파일이 있다면 `git checkout`은 일어날 수 없다.

## 그래서 왜 stash를 사용하는가?

변경사항이 있는데 잠시 어느 시점의 브랜치나 커밋으로 돌아가고싶을때 우리가 받은 에러 메시지 처럼 커밋을 하기에는 조금 부담감(?)이 있습니다 왜냐하면 보통 커밋은 완성된 한개의 기능, 한개의 모듈 단위로 하는데(정해진거 아님) 완성되지도 않은채로 커밋을 하기에는 찝찝하기 때문이다. 그럴때 우리는 이 변경사항을 `임시로` 저장할수 있으며 저장 후 checkout으로 다른 커밋과 브랜치를 자유롭게 왔다갔다 하다가 다시 돌아와 방금 저장한 임시저장내역을 다시 적용시키거나 심지어 같은 브랜치가 아닌 다른 브랜치에도 stash에 저장해놓은 내역을 적용시킬수 있다. 자 이제 여러가지 상황을 보았으니 이제 본격적으로 명령어 사용법을 알아보자.

## git stash

`git stash`명령어는 새로운 stash를 스택에 만들어 작업하던 `변경사항을 임시로` 저장한다. 변경사항을 임시로 저장한다라는 말을 잘 생각해보면 변경사항을 찾을 수 없는 파일, 즉 Git이 Untracked하는 파일은 `stash`하지 않는다. 커밋처럼 영구히 저장이 아닌 말그대로 임시로 저장을 해주는것 뿐이다. 물론 `git stash pop` 혹은 `git stash drop`하지 않는 이상 절대로 사라지지 않을것이다 하지만 `git log`명령어로 로그를 살펴보았을때 괜히 한개의 커밋만 더 추가되서 보일것이다. 그러니 로그를 깨끗하게 유지하기위해서라도 `stash`를 비워주는 것이 좋다.(의무는 아님)

[##_Image|kage@WQzBN/btq6pZnZ6OT/XbpoeyTwKvnDmNhTYRk3j0/img.png|alignCenter|width="100%" data-origin-width="619" data-origin-height="607" data-ke-mobilestyle="widthOrigin"|||_##]

나의 말이 이 사진으로 설명이된다. `git stash`를 하게되면 그 변경내용은 커밋처럼 하나의 해쉬값을 갖게 되므로 `git log`에 `refs/stash`라는 이름을 가진채 출력이 된다. 이런 `stash`된 내역들이 많이많이 쌓이고 커밋도 많이쌓이다보면 `git log`의 출력 내용은 겉잡을수 없이 알아보기 힘들어 질 것이다.

## git stash apply

`git stash apply`명령어는 스택에 담겨있던 0번째의 `stash`를 꺼내 적용시켜준다. 이는 stash를 저장할때 스택이라는 자료구조에 저장하기 때문이다. 하지만 특정 stash를 꺼낼수도 있는데 `git stash apply stash@{2}`처럼 2번째의 stash를 적용시킬수 있따. 또한 변경내역이 원래는 staged된 상태였다면 git은 staged 상태로 되돌리면서 적용시키지 않는다. 그래서 만약 staged된 상태까지 유지시키며 적용시키고 싶다면 `git stash apply --index`명령어를 통해 staged된 내용은 그대로 staged된 상태와 내용을 복구시킬수 있다. 그런데 apply명령어는 단순히 적용시킬 뿐이지 `stash`스택에서 방금 적용시킨 `stash`를 제거하지는 않는다. 따라서 `git stash drop`을 해주어야 스택에 저장된 `stash`가 사라지며 `drop`역시 `git stash drop stash@{2}`처럼 특정 `stash`를 `drop`할수 있다. 혹시 만약 적용과 동시에 삭제를 하고싶다면 `git stash pop`명령어를 사용하면 된다.

[##_Image|kage@cLbEGI/btq6lZo03B0/yLMffgndXHGlIfPLkJcp2k/img.png|alignCenter|width="100%" data-origin-width="444" data-origin-height="253" data-ke-mobilestyle="widthOrigin"|||_##]

## \--keep-index

\--keep-index 옵션으로 `staged`된 내용은 `stash`하지 않을수 있는데 단, 이러면 아직 해당 브랜치 혹은 커밋에 변경사항이 여전히 남아있기때문에 `checkout`하지 못한다. 아래사진 처럼 말이다.

[##_Image|kage@cG08XX/btq6oDTgY3w/lzpsEk5T1DcbxHEiLpN1pk/img.png|alignCenter|width="100%" data-origin-width="607" data-origin-height="553" data-ke-mobilestyle="widthOrigin"|||_##]

옵션의 이름이 왜 --keep-index인줄 아는가? `git-scm`에서는 staging영역을 index라고 하기때문에 인덱스는 지킨다 라는 뜻이 되므로 staging 영역의 변경사항은 `stash`스택에 올라가지 않는다.

## \--include-untracked 혹은 -u

위에서도 언급했듯이 `git stash`는 기본적으로 Tracked 상태의 파일만을 임시 저장 해주는데, Tracked상태의 파일 뿐만이 아닌 Untracked상태의 파일까지 `stash`하고 싶다면 위의 옵션을 붙혀주면 된다.

[##_Image|kage@b2XnEJ/btq6nBHYfb0/kAMERu1aRKZbaPwkm7bC20/img.png|alignCenter|width="100%" data-origin-width="611" data-origin-height="546" data-ke-mobilestyle="widthOrigin"|||_##]

다음과 같이 `git stash`는 Tracked 상태의 파일인 `one`, `two`파일만 stash 스택에 저장했으며 Untracked상태의 파일인 `five`는 `stash` 스택에 저장하지 않았다. 아래의 사진은 똑같은 상황을 만들고 `-u`옵션을 붙혔을때의 `git status`로 상태를 확인한 사진이다.

[##_Image|kage@dfooLg/btq6qgwgIVK/WirktcQXiOlSIs2Q18uTd1/img.png|alignCenter|width="100%" data-origin-width="608" data-origin-height="477" data-ke-mobilestyle="widthOrigin"|||_##]

이와 같이 `-u`옵션을 붙히면 Untracked 상태의 파일까지 `stash`스택에 저장해 준다.

## \--patch

`git stash --patch`는 변경된 모든 사항들을 저장하는 것이 아니며 대화형 프롬프트를 통해 자신이 stash에 저장할 것과 저장하지 않을 것을 지정할수 있다.

[##_Image|kage@cKLrG8/btq6mCt185y/ksnMzJw6nS60qKfZdsKk2k/img.png|alignCenter|width="100%" data-origin-width="602" data-origin-height="546" data-ke-mobilestyle="widthOrigin"|||_##]

# Stash를 적용한 브랜치 만들기

보통 Stash에 저장하면 한동안 그대로 유지한 채로 그 브랜치에서 새로운 일을 계속 한다. 그러면 여태 수정해왔던 파일에 다시 stash를 적용하려고 한다면 충돌이 생길수 있다. 그러면 이 충돌을 또 해결해야하는 번거로움이 생기는데 이럴땐 `stash`를 한 시점에 새로운 브랜치를 만들어주고 그 브랜치로 `checkout`후 적용까지 해줄수 있는데 `git stash branch <브랜치>`로 이러한 기능을 이용할수 있다. 만약 명령이 성공하면

[##_Image|kage@njsH6/btq6qh222Z3/CgVMHkoYknZbPUjGQehymK/img.png|alignCenter|width="100%" data-origin-width="704" data-origin-height="628" data-ke-mobilestyle="widthOrigin"|||_##]

`one`, `two`, `three`의 3개의 파일을 새로 만들고 커밋 한개를 만들었다. 그리고 `one`파일에 변경사항 `one`이라는 텍스트로 변경사항을 주고 커밋하지 않고 `stash`스택에 담았다. 그리고는 몇개의 변경사항을 주고 커밋을 했으며 이제 `git stash branch develop`이라는 명령어로 `stash`했던 시점인 `4ca5a181`커밋에 `develop`브랜치를 생성하고 `develop` 브랜치로 `checkout`하여 `stash`했던 내용인 `one`이라는 내용이 정상적으로 적용까지 시켜지는지 볼 것이다. 아래는 명령을 실행하기 이전의 상태이다. `one`이라는 파일은 `abcd`라는 내용을 가지고있다.

[##_Image|kage@mVyJg/btq6sdZTe9b/DD69k9ePyEpDwekDzlXcs1/img.png|alignCenter|width="100%" data-origin-width="706" data-origin-height="277" data-ke-mobilestyle="widthOrigin"|||_##]

결과는 예상과 정확하게 맞아 떨어졌다. 결과도 중요하지만 명령어 바로 아래의 `Switched to a new branch 'develop'`와 `Dropped refs/stash@{0} (adfd53247bc3c17e6a9dbcd18879ec0219230039)`을 통해 `git stash branch`명령어로 했던 모든 명령들을 한눈에 보여주고 있다.

[##_Image|kage@YduI2/btq6mFRpExq/Z0akPexALnzx7kiRDfocv0/img.png|alignCenter|width="100%" data-origin-width="713" data-origin-height="412" data-ke-mobilestyle="widthOrigin"|||_##]

정말 어마어마한 기능인것 같다. 단순히 버전관리를 넘어 이건 뭐 깃을 점점 알면 알수록 관리를 못할게 없어질것 같다. 또한 아래 사진에서는 `stash`스택에 있던 내용까지 `drop`된것 까지 알수 있다.

[##_Image|kage@cfn84j/btq6p9qBHx7/y2MjzkmYqmuLW8f2VsoIQK/img.png|alignCenter|width="100%" data-origin-width="710" data-origin-height="54" data-ke-mobilestyle="widthOrigin"|||_##]

# 참고

[Git - Stash](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Stashing%EA%B3%BC-Cleaning)
