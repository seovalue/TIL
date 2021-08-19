# Git Submodule

## About Git Submodule
서브모듈이란 하나의 저장소 안에 있는 또 다른 별개의 저장소이다.
보통 다른 원격 저장소를 pull 해 온 뒤 서브모듈로서 사용하게 된다.

## How to link
예를 들어, 게시판을 만들고 있는 board라는 프로젝트 디렉토리에서 게시판을 개발하던 중 채팅 모듈을 원격 저장소에서 가져와 게시판 프로젝트에서 사용한다고 가정하자.

채팅 모듈의 원격 저장소 링크: https://fakegit.com/chat.git (가안)

board 디렉토리에서, 채팅 모듈을 서브모듈로서 사용하기 위해 다음 명령어를 사용한다.

```
git submodule add https://fakegit.com/chat.git 
```

그럼 .gitmodules 파일과 함께 chat/이 생성되어 스테이징 된다.

.gitmodules 파일을 열어보면 다음과 같이 서브모듈에 대한 정보가 적혀있다.
```
//.gitmodules

[submodule "chat"]
    path = chat
    url = https://fakegit.com/chat.git
```

그럼 내가 채팅 모듈을 추가한 게시판 프로젝트를 다른 B라는 개발자가 clone해와서 쓸 때, 채팅 모듈도 함께 가져오고 싶으면 어떻게 해야할까?

기존대로 `git clone ~~`을 진행하게 되면, 서브모듈은 가져와지지 않는다.
왜냐하면, 메인 프로젝트 입장에서 서브모듈은 단지 현재 가리키는 커밋과 변경 여부만 적혀있는 하나의 파일에 불과하기 때문이다.

따라서 서브모듈을 가져오기 위해서는 2가지 명령어를 추가로 실행해주어야한다.
```
git submodule init
git submodule update
```

또는, clone 해올 때 처음부터 submodule과 관련된 옵션을 주면 한번에 서브모듈까지 가져올 수 있다.
```
git clone --recurse-subdmoules https://fakegit.com/board.git
```

## Example
보또보 팀에서는 서브모듈을 민감 정보를 업로드하는 용도로 사용했다. 깃허브의 public repository에는 .gitignore에 포함되지 않는다면 모든 정보가 다 업로드 되게 되어있다. 그리고 만약, 이미 민감 정보를 커밋해버렸다면 이후 지웠다 하더라도 해당 커밋 내역을 통해 그 정보를 확인할 수 있다. 🚨

따라서 aws 정보나, configuration 파일에 민감한 정보가 포함되어있다면 submodule을 활용해서 보관할 수 있다.

### 민감 정보를 보관할 Private Repository를 생성한다.

이 예시에서 사용될 별칭을 설명하자면 다음과 같다.
> 보또보: 메인 프로젝트  
> 보또보/resources: 서브모듈을 추가하고 싶은 디렉토리  
> 민감정보: 민감 정보가 포함된 서브모듈명  
> 민감정보/민감.yml: 민감 정보가 포함된 파일

민감 정보를 보관할 private Repository(민감정보)를 생성한 뒤, 위에서 진행했던 것 처럼 기존의 내 프로젝트 디렉토리 중 해당 민감정보를 포함할 디렉토리인 보또보/resources로 이동한 뒤 git submodule add를 실행한다.

이후 메인 프로젝트인 보또보의 .gitignore 파일에 민감 정보 관련 파일을 포함한다. (메인 프로젝트에 민감정보를 올리지 않도록 하기 위함)

보또보에서 민감정보를 가져와서 사용하기 위해서는 다음 명령어를 수행하면 된다.
```
git submodule update --remote --merge --init
```

Spring 프로젝트를 사용하고 있는데, resources/application.yml 파일이 민감 정보라 서브모듈에 포함되어있고, 매 빌드마다 resources 내부로 해당 파일을 옮겨줘야한다면 build.gradle에 다음과 같이 작성하면 된다.

```
processResources.dependsOn('copy민감')

task copy민감(type: Copy) {
    from '민감정보/민감.yml'
    into './src/main/resources'
}
```

## Alias
submodule과 관련된 명령어는 매우 길어서 외우기가 힘들다.
다음과 같은 별칭을 등록해놓으면 편리하게 사용할 수 있다.
```
git config alias.supdate 'submodule update --remote --merge --init'
```

## References
* [Git 의 서브모듈(Submodule) - Sungho’s Blog](https://sgc109.github.io/2020/07/16/git-submodule/)
* [Git - 서브모듈](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-%EC%84%9C%EB%B8%8C%EB%AA%A8%EB%93%88)
