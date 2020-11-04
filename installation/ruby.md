# Ruby

기존에는 ['멘붕없이 rvm과 루비 설치하기'][1]을 보고 rvm이 설치되어 있었다. 그러다가 ['초레가 - rbenv 설치'][2]를 보고는 [rbenv][]로 변경해보자 하였다. 결론은 프로젝트별로 `rbenv local`을 통해 ruby 버전을 관리할 수 있는 rbenv에 손을 들어주고 싶다. 사실 [rvm][]도 익숙하지 않은 상태에서 rbenv으로 변경하여 장단점을 다 파악한건 아니지만 말이다. ruby 버전별로 gem이 따로 설치 되는 관계로 local이 지정된 디렉토리에서 '왜 gem이 없다고 그러지?'하는 착각을 했던 그런 어처구니 없는 일도 있었더랬다.  

## rvm 제거

rbenv는 rvm과 호환되지 않으므로 rvm이 설치되어 있다면 지워줘야 한다.

```
$ rvm implode
```

본인은 rvm 설치시 sudo로 설치한 관계로 위와 같이 실행했더니 permission denied가 뜨며, 일부 폴더가 지워지지 않는 사태가 발생하였다. 이런 경우에는 `rvmsudo`를 이용하면 해결될 듯 하다. (본인은 그냥 지웠다.)

```
$ rvmsudo rvm implode
```

## rbenv 설치

#### Homebrew on MAC OS X

맥에서는 [homebrew][]를 통해 쉽게 rbenv를 설치 할 수 있다.

```shell
$ brew update
$ brew install rbenv ruby-build
```

rbenv를 설치하고 `rbenv init -`을 실행하면 다음 일련의 과정을 수행함을 볼 수 있다. 간략히 말하면 `~/.rbenv/shims` 경로의 명령어들을 사용할 수 있게 하는 것이다.

1. shims 경로 설정
2. 자동완성 설치
3. Rehashes shims
4. Installs the sh dispatcher

`rbenv init -`의 내용을 bash 시작시마다 실행해줘야 하므로 다음과 같이 `.bash_profile`에 설정한다.

```shell
$ echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
```

#### Linux

리눅스의 경우 github 저장소를 복사하여 설치한다.

```shell
$ git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
```

그리고 `~/.rbenv/bin`을 `$PATH`에 추가하고, `rbenv inin`도 위에서와 같이 추가한다.

```shell
$ echo 'export PATH="${HOME}/.rbenv/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
```

쉘을 재시작하거나 `source ~/.bash_profile`을 실행하여 환경변수를 갱신한다.

```shell
$ type rbenv
rbenv 는/은 함수임
```

## ruby 설치

#### ruby-build 설치

[`rbenv install`](#ruby-install)이나 [`rbenv uninstall`](#ruby-uninstall) 명령은 rbenv에 의해 제공되지 않는다. 이를 위해서는 [`ruby-build`][ruby-build] 플러그인이 필요하다. rbenv 플러그인들은 *~/.rbenv/plugins* 디렉토리에 설치하면 된다.

```shell
$ mkdir -p ~/.rbenv/plugins
$ cd ~/.rbenv/plugins
$ git clone https://github.com/sstephenson/ruby-build.git
```

#### ruby install

설치된 ruby는 `~/.rbenv/versions` 디렉토리 하위에 저장된다.

```shell
# 설치 가능한 모든 ruby 버전들
$ rbenv install --list

# ruby 버전 설치
$ rbenv install 2.2.2
```

#### ruby uninstall

ruby를 지우는 방법은 `rm -rf`로 해당하는 디렉토리를 직접 지우는 방법과 `rbenv uninstall`을 이용한 방법이 있다. `rbenv prefix`로 해당하는 디렉토리를 구할 수 있다.

```shell
$ rbenv prefix 2.2.2
/Users/go2zo/.rbenv/versions/2.2.2
$ rm -rf /Users/go2zo/.rbenv/versions/2.2.2

# or

$ rbenv uninstall
```

## ruby 버전 관리

#### rbenv versions

시스템에 설치되어 있는 ruby 버전 리스트를 보여준다.

```shell
$ rbenv versions
  2.1.6
* 2.2.2 (set by /Users/go2zo/.rbenv/version)
```

#### rbenv version

현재 디렉토리에서 사용하는 ruby의 버전을 보여준다. `rbenv local`이 설정되어 있으면 해당 버전을 사용하고 그렇지 않다면 global 버전을 사용한다.

```shell
# Not set 'rbenv local'
test $ rbenv version
2.2.2 (set by /Users/go2zo/.rbenv/version)

test $ rbenv local 2.1.6

test $ rbenv version
2.1.6 (set by /Users/go2zo/git-repo/test/.ruby_version)
```

#### rbenv local

현재 디렉토리에서 사용할 ruby버전을 설정한다. 설정된 내용은 `.ruby-version` 파일에 저장되어 있다.  

```shell
$ rbenv local 2.1.6

$ rbenv local
2.1.6

$ rbenv global
2.2.2

$ cat .ruby_version
2.1.6
```

local 설정을 해제하기 위해서는 `--unset` 옵션을 사용하면 된다.

```shell
$ rbenv local --unset

$ rbenv local
rbenv: no local version configured for this directory
```

#### rbenv global

시스템에서 사용할 ruby 버전을 설정한다. MAC의 경우에는 기본으로 설치된 ruby가 있으므로 `system`이 초기값이다.

#### rbenv shell

#### rbenv rehash

새로운 ruby나 gem을 설치한 후에는 반드시 `rbenv rehash`를 실행해야한다. `rbenv init -`과 거의 동일한 기능으로 환경을 재설정한다.

## ruby 플러그인

* [rbenv-gem-rehash][]를 설치하면 매번 `rbenv rehash` 명령을 실행하지 않아도 된다.
* [rbenv-bundler][]를 설치하면 명령 앞에 `bundle exec`를 생략해도 된다.

[1]: http://bigmatch.i-um.net/2013/12/%EB%A9%98%EB%B6%95%EC%97%86%EC%9D%B4-rvm%EA%B3%BC-%EB%A3%A8%EB%B9%84-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0/
[2]: http://rorlab.gitbooks.io/railsguidebook/content/contents/rbenv.html
[rbenv]: https://github.com/sstephenson/rbenv
[rvm]: https://rvm.io/
[homebrew]: http://brew.sh/
[ruby-build]: https://github.com/sstephenson/ruby-build
[rbenv-gem-rehash]: https://github.com/sstephenson/rbenv-gem-rehash
[rbenv-bundler]: https://github.com/carsomyr/rbenv-bundler