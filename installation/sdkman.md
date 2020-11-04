# SDKMAN

SDKMAN은 java 외에도 웬만한 sdk는 모두 multi version을 관리할 수 있는 도구이다.

GVM이나 RVM, rbenv 등의 도구에서 영감을 얻었다는데 groovy, ruby도 모두 지원한다.

기존에 java 버전 관리로 cask+jenv 조합을 사용했었는데 활발한 업데이트를 보여주는 `SDKMAN`으로 갈아탄 상태이다.

## Installation

UNIX-like 플랫폼에서 아래와 같이 쉽게 설치가 가능하다.

Mac OSX, Linux, Cygwin, Solaris, FreeBSD에서 원활하게 설치된다.

또한 Bash, ZSH 쉘도 지원한다.

```sh
$ curl -s "https://get.sdkman.io" | bash
```

설치가 완료되면 새로 터미널을 열거나 아래와 같이 실행한다.

아래 경로는 설치 기본경로이며 경로의 변경은 아래 `Installing to a Custom Location`를 참조한다.

```sh
$ source "$HOME/.sdkman/bin/sdkman-init.sh"
```

아래와 같이 입력하여 설치가 잘 되었는지 확인한다.

```sh
$ sdk version
SDKMAN 5.5.12+269
```

### Update

sdkman 자체의 업데이트는 아래와 같이 실행한다.

sdkman을 실행하는 과정에서 자동으로 업데이트 여부를 묻기도 한다. (메인버전이 변경된 경우일 듯 하다.)

```sh
$ sdk selfupdate [force]
```

beta channel을 통해 beta version을 사용할 수도 있다.

`.sdkman/etc/config` 파일에서 아래와 같이 수정한다.

```properties
sdkman_beta_channel=true
```

### Uninstallation

sdkman의 삭제를 자동으로 해주는 것은 없고 `.sdkman` 디렉토리만 삭제하면 된다.

`.sdkman` 디렉토리를 백업했다가 복구하여 재설치가 가능하므로 백업을 해두자.

```sh
$ tar zcvf ~/sdkman-backup_$(date +%F-%kh%M).tar.gz -C ~/ .sdkman
$ rm -rf ~/.sdkman
```

그리고 `.bashrc`, `.bash_profile` 혹은 `.profile` 파일에 아래와 같이 추가된 스크립트를 삭제(주석처리)한다.

ZSH는 `.zshrc` 파일이 해당한다.

```shell
#THIS MUST BE AT THE END OF THE FILE FOR SDKMAN TO WORK!!!
[[ -s "/home/dudette/.sdkman/bin/sdkman-init.sh" ]] && source "/home/dudette/.sdkman/bin/sdkman-init.sh"
```

### Installing to a Custom Location

기본 설치경로인 `$HOME/.sdkman` 대신 다른 경로를 지정할 수 있다.

환경변수 `SDKMAN_DIR`을 지정하여 설치하는 방법이다.

그런데 해당 경로에 대해 권한이 없으면 아마 `sudo`를 붙여서 실행해야 할 것이다.

또한 sdk를 설치할 때도 root 권한이 항상 필요할 것이다.

```sh
$ export SDKMAN_DIR="/usr/local/sdkman" && curl -s "https://get.sdkman.io" | bash
```



## Usage

`sdk list` 명령으로 전체 SDK 목록을 볼 수 있다.

터미널에서 보긴 사실 불편하고 지원하는 SDK를 찾아보기 위해서는 <https://sdkman.io/sdks>를 참조한다.

### Installing an SDK

아래와 같이 `install` 명령을 이용하며 `version`을 명시하지 않으면 **latest stable** 버전을 설치한다.

```sh
$ sdk install java [version]
```

버전 정보를 확인하기 위해서는 `list` 명령을 이용한다.

`list`는 SDK를 입력하지 않으면 전체 목록을, SDK를 선택하면 해당 SDK의 선택가능 버전 목록을 보여준다.

설치되거나 현재 사용중인 버전도 확인할 수 있다.

```sh
$ sdk list java
================================================================================
Available Java Versions
================================================================================
     9.0.1-oracle
     9.0.0-zulu
 > * 8u151-oracle
     8u144-zulu
     8u131-zulu
     7u141-zulu
     6u65-apple
================================================================================
+ - local version
* - installed
> - currently in use
================================================================================
```

### Use Version vs Default Version

`use` 명령은 현재 shell에만 효력을 발휘한다.

터미널을 새로 열 경우 `use`로 지정한 부분은 이전으로 돌아간다.

```sh
$ sdk use java 8u151-oracle
```

반면 `default` 명령은 기본적인 버전을 변경해주어 터미널을 새로 열어도 설정한 값이 반영된다.

```sh
$ sdk default java 8u151-oracle
```

### Remove Version

```sh
$ sdk uninstall java 8u151-oracle
```



## Postrequisite

### Oracle Java

oracle java 설치 시 `/Library/Java/JavaVirtualMachines/`에 jdk가 함께 설치된다.

이는 인스톨러나 cask가 설치하는 경로와 동일하다.

IntelliJ 등의 IDE에서는 이 경로를 사용하는 것이 좋을 듯 하다.

재 설치시에 위의 경로에 생성되지 않는 다면 `flush` 명령으로 캐시를 삭제한다.

```sh
$ sdk flush temp
$ sdk flush archives
```

`/Library/Java/JavaVirtualMachines/`에 따로 설치한다고 sdkman으로 관리되진 않는다.

### Alternative Java

OpenJdk, AdoptOpenJdk, Zulu 등은 기본적으로 `~/.sdkman/candidates/java` 하위에 설치된다.





## Trouble-shooting

### .sdkman/var/candidates no such file or directory

`sdk flush`를 실행 한 후로 터미널 실행 시 위와 같은 메시지가 뜬다.

그 시점에 실행되는 스크립트는 `sdkman-init.sh` 이며 아무래도 초기화 시 해당 파일에 접근하는 듯 하다.

결국 위 파일을 생성해 줘야 하는데 어떤 내용을 채워주는지가 문제다.

다행히 구글링을 통해 알 수 있었다. (API 버전이 2가 되며 경로가 변경된 듯 하다.)

```bash
$ curl -s https://api.sdkman.io/2/candidates/list > ~/.sdkman/var/candidates
```

이와 함께 `sdk list java` 실행 시 `404 Not Found` 에러 뜨는 문제도 같이 해결되었다.

