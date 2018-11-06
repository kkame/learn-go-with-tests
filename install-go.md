# Go 설치 및 생산성 향상을 위한 환경설정 셋업

Go의 공식 설치 지침은 [여기](https://golang.org/doc/install)에서 볼 수 있습니다.

이 가이드는 당신이 패키지 관리자를 사용 중이라고 가정합니다. 예를 들어 [Homebrew](https://brew.sh), [Chocolatey](https://chocolatey.org), [Apt](https://help.ubuntu.com/community/AptGet/Howto) 또는 [yum](https://access.redhat.com/solutions/9934) 같은 것이 있습니다.

데모로 Homebrew를 사용하는 OSX의 설치 절차를 보여줍니다.

## 설치

설치 과정은 매우 쉽습니다. 먼저, homebrew (brew)를 설치하기 위해이 명령을 실행해야합니다. Brew는 Xcode에 대한 의존성이 있으므로 Xcode를 먼저 설치해야합니다.

```sh
xcode-select --install
```

그다음 아래의 명령어을 실행하여 homebrew를 설치하십시오.

```sh
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

이제 GO를 설치 할 수 있습니다

```sh
brew install go
```

Linux 기반 서버에 프로그램을 배포하려면 크로스 컴파일 기능을 사용해야합니다. 그렇다면 다음 명령을 사용하여 설치하십시오.

```sh
brew install go --cross-compile-common
```

*패키지 관리자가 권장하는 지침을 따라야합니다. **참고** 이들은 호스트 OS에 따라 다를 수 있습니다*.

다음을 사용하여 설치를 확인할 수 있습니다.

```sh
$ go version
go version go1.10 darwin/amd64
```
### 역자주
개인적으로는 Ubuntu에서 Go를 개발하고 있으며 [Go Version Manager](https://github.com/moovweb/gvm)를 사용하고 있습니다.
이 편이 고의 다양한 버전에서 개발하기가 개인적으로는 조금 더 수월 하고 신 버전이 출시되었을 때에 대한 부담이 적었습니다.
만약 고의 다중 버전 사용에 대해 고민하신다면 한번쯤 고려해보시는 것도 괜찮습니다.
샘플 :
```sh
$ gvm install go1.11.2
$ gvm use go1.11.2 --default
$ go version
go version go1.11.2 linux/amd64
```

## Go 환경 설정

Go는 빡빡합니다(opinionated).

Go의 모든 코드는 규칙에 따라 단일 작업 공간 (폴더) 내에 있어야 합니다. 이 작업 공간은 컴퓨터의 어느 곳이든 상관없습니다. 단 지정하지 않으면 Go는 $HOME/go를 기본 작업 공간으로 간주합니다. 작업 공간은 환경 변수 [GOPATH](https://golang.org/cmd/go/#hdr-GOPATH_environment_variable)에 의해 식별되고 수정됩니다.

나중에 스크립트, 쉘 등에서 환경 변수를 사용할 수 있도록 환경 변수를 설정해야합니다.

다음 export문을 .bash_profile에 포함시키세요.

```sh
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

*참고* 이러한 환경 변수를 사용하기 위해 새 쉘을 열어야합니다. (역자주: 개인적으로는 .bashrc 또는 .zshrc 파일에 추가하는 편이며 바로 해당 변수 사용을 위해서 source ~/.zshrc 같은 명령어 입력 후 사용합니다)

Go는 작업 공간에 특정 디렉토리 구조가 있다고 가정합니다.

Go는 파일을 세 개의 디렉토리에 저장합니다. 모든 소스 코드는 src에 있고 패키지 객체는 pkg에 있으며 컴파일 된 프로그램은 bin에 있습니다. 다음과 같이 이 디렉토리를 작성할 수 있습니다.

```sh
mkdir -p $GOPATH/src $GOPATH/pkg $GOPATH/bin
```

이 시점에서 _go get_ 및 src / package / bin이 해당 $GOPATH/xxx 디렉토리에 올바르게 설치됩니다.

## Go 에디터

에디터 환경 설정은 매우 개인적인 스타일이므로 해당하는 에디터에 이미 Go를 지원하는 환경 설정이 있을 수 있습니다. 아니라면 Go Support가 있는 [Visual Studio Code](https://code.visualstudio.com)와 같은 에디터를 고려해 보시길 바랍니다.

brew를 사용하여 VS Code를 설치하려면 VS Code는 GUI 프로그램이라서 cask라는 homebrew extension이 필요합니다.

```sh
brew tap caskroom/cask
```

이제 brew를 사용하여 VS Code를 설치할 수 있습니다.

```sh
brew cask install visual-studio-code
```

다음에 나오는 쉘 스크립트를 이용해서 VS Code가 정상적으로 설치되었는지 확인하고 실행 할 수 있습니다.

```sh
code .
```

VS Code는 매우 적은 기능이 활성화 된 상태로 설치됩니다. 확장 기능을 설치하여 새로운 기능을 활성화 할 수 있습니다. Go 지원 기능을 추가하려면 확장 기능을 설치해야합니다. VS Code에서 사용할 수 있는 다양한 기능이 있습니다. [Luke Hoban의 패키지](https://github.com/Microsoft/vscode-go)가 대표적입니다. 다음과 같이 설치할 수 있습니다. (역자주: 깃헙주소는 마이크로소프트 인데 왜 원문에서는 Luke Hoban을 명시해뒀는지 모르겠네요)

```sh
code --install-extension ms-vscode.go
```

VS Code에서 처음으로 Go 파일을 열면 분석 도구가 없음을 나타내며 이를 설치하려면 버튼을 클릭해야합니다. VS Code로 설치되고 사용되는 도구 목록은 [여기](https://github.com/Microsoft/vscode-go/wiki/Go-tools-that-the-Go-extension-depends-on)에서 사용할 수 있습니다.

## Go 디버거

(VS 코드와 통합 된) Go의 디버깅을 위한 좋은 옵션은 Delve입니다. 이것은 다음과 같이 go get을 사용하여 설치할 수 있습니다.

```sh
go get -u github.com/derekparker/delve/cmd/dlv
```

## Go Linting

기본 linter보다 향상된 기능은 [Gometalinter](https://github.com/alecthomas/gometalinter)를 사용하여 설정 할 수 있습니다.

다음과 같이 설치할 수 있습니다.

```sh
go get -u github.com/alecthomas/gometalinter
gometalinter --install
```

## 리팩토링 및 툴 사용

이 책의 큰 강조사항은 리팩토링의 중요성입니다.

도구를 사용하면 더 큰 리팩토링을 자신있게 수행 할 수 있습니다.

간단한 키 조합으로 다음을 수행하려면 에디터에 익숙해야합니다.

- **변수의 추출 및 집어넣기**. 마법 값을 가져 와서 이름을 부여하면 코드를 신속하게 단순화 할 수 있습니다. (역자주: 번역하기가 애매한데 타이틀만 봐도 의미를 이해하는데는 큰 문제는 없을 것 같습니다)
- **메서드/함수의 추출**. 코드 섹션을 가져 와서 함수 / 메소드를 추출 할 수 있어야합니다.
- **이름 바꾸기**. 파일간에 심볼의 이름을 확신을 가지고 바꿀 수 있어야합니다.
- **go fmt** . Go는 `go fmt` 라고 불리는 포매터를 가지고 있습니다. 편집자는 모든 파일은 저장 할 때 이것을 실행해야합니다. (역자주: 항상 Go에서 권장하는 포맷을 유지해야합니다)
- **테스트 실행**. 위의 작업 중 하나라도 수행 할 수 있어야하며 리팩토링이 아무 것도 손상시키지 않도록 테스트를 신속하게 다시 실행할 수 있어야합니다.

또한 코드 작성에 도움이되도록 다음을 수행 할 수 있어야합니다.

- **함수 시그니처 보기** - Go에서 함수를 호출하는 방법을 확신 할 수 있어야합니다. IDE는 함수의 문서, 파라메터 및 반환되는 항목과 관련하여 설명해야합니다.
- **함수 정의보기** - 함수가 무엇인지 아직 명확하지 않은 경우 소스 코드로 이동하여 직접 알아낼 수 있어야합니다.
- **심볼의 사용법 찾기** - 호출되는 함수의 컨텍스트를 볼 수 있으며 리팩토링 할 때 의사 결정 과정을 도울 수 있습니다.

도구를 마스터하면 코드에 집중하고 컨텍스트 전환을 줄일 수 있습니다.

## 마무리

이 시점에서 Go 설치, 사용 가능한 에디터 및 일부 기본 도구를 설치를 완료 할 수 있습니다. Go는 서드파티 제품의 매우 큰 생태계를 가지고 있습니다. 좀 더 자세한 목록은 https://awesome-go.com에서 몇 가지 유용한 구성 요소를 확인할 수 있습니다
