
## Submodule을 사용중인 저장소 clone 할 때.

Submodule이 비어있는 경우, 아마도 해당 submodule의 내용을 가져오지 않았을 가능성이 있습니다. Submodule은 별도의 저장소(repository)로 구성되어 있으며, 기본적으로 메인 저장소에서는 submodule의 내용을 함께 가져오지 않습니다. 따라서 submodule을 초기화하고 내용을 가져와야 합니다.

다음은 submodule을 초기화하고 내용을 가져오는 몇 가지 단계입니다:

1. submodule 초기화: submodule을 초기화하여 .gitmodules 파일에 있는 정보를 사용하여 submodule에 필요한 설정을 가져옵니다.

```bash
git submodule init
```

2. submodule 업데이트: submodule의 내용을 가져옵니다.

```bash
git submodule update
```

이 명령어들을 순서대로 실행하면 submodule의 내용이 가져와질 것입니다. 만약에도 submodule이 비어있는 경우, submodule 저장소가 올바른지 확인하고, submodule 저장소에 대한 액세스 권한이 있는지 확인하세요.


## Submodule 제거 방법
- https://snowdeer.github.io/git/2018/08/01/how-to-remove-git-submodule/
