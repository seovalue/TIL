# Git branching이란

## Why git?
* 편리한 버전 컨트롤
* 협업 툴
* powerful branching

### Version Control
파일을 관리하게 되면 이름을 통해 관리하게 된다.
ex) 조앤_최종, 조앤_최최종, 조앤_최최최종
이런 식으로 관리하게 되면 어느 시점에 작성한 파일인지 확인하기가 어렵고, 파일의 제목만 수정해서 전달해야할 경우에도 불편하다. 즉, 파일의 버전을 다루는 것이 불편하다.

### Co-working Tool
깃으로 협업함으로서 각각 어떤 일을 하고 있는지 커밋 로그를 통해 변경사항을 한눈에 확인할 수 있어 어떤 업무를 담당하고 있는지 확인하는 것이 편리하다.

### Powerful Branching
여러 프로젝트에서 관심사를 나눠가지고, 프로젝트 매니징을 위한 주요한 개발도구로서 사용할 수 있다. 선형적으로 관리하는 것이 아니라 여러 갈래로 나누어 (여러 브랜치를 통해) 사용할 수 있기 때문에 관심사를 분리하는 것이 가능하다.

## Github-flow

![Github-Flow](https://subicura.com/git/assets/img/github-flow.2fafce92.png)

1개의 master 브랜치와 PR을 활용한 단순하고 민첩한 브랜치 전략. 마스터 브랜치는 제품이 릴리즈되는 가장 최신 버전의 브랜치이며, 모든 개발 내용이 마스터 브랜치를 중심으로 이루어진다.

이렇게 말하면 헷갈리는데, 간단히 설명해보자면 예를 들어 줄넘기 기능을 만든다고 가정해보자.
master 브랜치에서 jumprope 브랜치를 생성한다.
jumprope 브랜치에서 필요한 파일을 추가하고, 커밋한다.
이때, 커밋 로그는 유의미하므로 자세한 커밋 로그를 남기자.
이후 jumprope 브랜치에서 의미있는 작업이 완료되었으며 merging 준비가 끝났다면, 마스터 브랜치를 향해 Pull Request를 보낸다.
충분한 토론 끝에 Pull Request가 통과되면 이를 머지하고 자동으로 배포한다.

## git-flow
![git-flow](https://user-images.githubusercontent.com/37354145/110271699-e894e600-800b-11eb-9b96-d89a29033f5d.png)

git-flow는 5가지의 브랜치를 이용해서 저장소를 운영하는 브랜치 전략이다. 5가지 중 항시 유지되는 메인 브랜치인 master, develop 2가지와 머지되면 사라지는 보조 브랜치인 feature, release, hotfix로 구성된다.

* master : 상용에 배포되는 브랜치.
* develop : 개발 서버용 브랜치.
* feature : 기능 개발 브랜치. develop 브랜치로 머지된다.
* release : 다음 버전 출시를 준비하는 브랜치. develop 브랜치를 release 브랜치로 옮긴 후 QA, 테스트를 진행하고 master 브랜치로 합친다.
* hotfix : master 브랜치에서 발생한 버그를 수정하는 브랜치.


## 우리 팀의 브랜칭 전략
우리 팀은 현재 다음과 같은 브랜치들로 구성되어있다.
* main: 상용에 배포되는 브랜치
* develop: 개발 서버용 브랜치
* feature: 기능 개발 브랜치
* fix: 개발 서버에서 테스트시 버그사항이 발견될 경우 생성해서 develop으로 머지한다.

### 보완해야할 점
`release` , `hotfix`브랜치를 적극 활용해야한다.

현재는 develop에서 main으로 배포될 때 squash-merge를 통해 배포하며, 개발 서버에서 미처 확인하지 못한 버그가 상용에서 발견되는 경우에는 hotfix라는 커밋 prefix를 단 커밋을 이용해 main에서 hotfix를 처리했다.

하지만 이 방법보다는 release 브랜치를 생성한 뒤, develop에서 main으로 배포되기 전 release 브랜치에 배포 후 충분한 QA와 test를 진행한 뒤 통과하면 main으로 머지하는 방식을 취해야한다고 생각한다.  
그 이유는 현재 develop에서 main으로 한번에 머지가 안되면 메인에 머지할 때 conflict를 해결해주어야하는 과정이 존재하기도 했고, main으로 머지할 때 변경사항이 많기 때문에 테스트해볼 곳이 존재하지 않아 머지 후에야만 알 수 있는 버그들이 존재했기 때문이다.
main브랜치는 상용 브랜치이므로 최대한 최신화, 안정성을 갖도록 하고 변경사항에 대한 대비는 release에서 하는 것이 맞다고 생각한다.

또한 hotfix도 커밋 메시지에 hotfix prefix를 다는 것이 아니라 hotfix 브랜치를 생성해서 어떤 부분에서 버그상황이 있었고 어떤 식으로 변경되었는지 팀원들이 확인하고 main으로 머지하는 것이 좋다고 생각한다.
