## Linux



## macOS



## Windows

### Chocolatey 통한 설치

#### `choco` 설치

- [`choco` 설치](/installation/chocolatey.md#installation) 참조

#### Node JS LTS 설치

[chocolatey에서 패키지를 검색](https://chocolatey.org/packages/nodejs-lts)하여 설치한다.

```powershell
> choco install nodejs-lts
```

#### 설치 확인

```powershell
> node -v
v12.16.3
> npm -v
6.14.4
```

### Windows Installer 통한 설치

chocolatey package 설치 시 인스톨러가 제공되는 경우 기본적으로 인스톨러 설치가 된다.

따라서 [링크](https://nodejs.org/ko/download/)에서 최신 LTS 버전 msi 파일을 다운로드하여 설치하여도 동일한 결과이다.