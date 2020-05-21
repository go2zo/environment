# Install NodeJS on Unix-like

## Using `n` package {#using-n-package}

[`n`](https://github.com/tj/n#installation)은 [`nvm`](https://github.com/nvm-sh/nvm#install--update-script) 같은 node 버전관리 패키지이다. (Windows 불가)

> `n` 설치를 위해서는 **npm**이 설치되어 있어야 하며, npm은 node 인스톨 시 함께 설치된다.
> 하지만 이렇게 설치된 node와 n으로 설치한 node간에 충돌이 발생하는 경우가 있다.
> 가장 깔끔한 방법은 npm과 n 설치 후 node를 설치하는 방법으로 생각된다. 

### `npm` 설치

[npm 설치 스크립트](https://github.com/npm/cli#fancy-install-unix)를 지원한다.

```bash
$ curl -L https://www.npmjs.com/install.sh | sh
```

### `n` 설치

```bash
$ npm install -g n
```

### node 설치

LTS 버전을 확인하고 설치한다.

```bash
$ n --lts
12.16.3
$ n lts
```

## Using Package Manager

공식 Node.js 바이너리 배포판은 [NodeSource](https://github.com/nodesource/distributions/blob/master/README.md)가 제공한다.

> 기타 OS에 대한 안내는 [공식문서](https://nodejs.org/ko/download/package-manager/)를 참조하라.

### Debian and Ubuntu based

**NodeJS v14.x:**

```bash
# Using Ubuntu
$ curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
$ sudo apt-get install -y nodejs

# Using Debian, as root
$ curl -sL https://deb.nodesource.com/setup_14.x | bash -
$ apt-get install -y nodejs
```

### Enterprise Linux based

**NodeJS 14.x**:

```bash
# As root
$ curl -sL https://rpm.nodesource.com/setup_14.x | bash -

# No root privileges 
$ curl -sL https://rpm.nodesource.com/setup_14.x | sudo bash -
```

## Using [`nvm`](https://github.com/nvm-sh/nvm)

### `nvm` 설치

[설치 스크립트](https://github.com/nvm-sh/nvm#install--update-script)를 이용하여 설치한다.

```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
```

### node 설치

```bash
# Long-term Support 설치
$ nvm install --lts

# latest
$ nvm install node

# specific version
$ nvm install 10.10.0
```

## Using Source Code

조금 복잡한 설치 과정으로 인해 본인은 시도하지 않은 방법이며 [문서](https://github.com/nodejs/help/wiki/Installation)로 설명을 대체한다.

# Uninstall

[macOS#Uninstall](install-nodejs-on-macos.md#uninstall)을 참조하대 linux도 동일한지는 불명확하다.

