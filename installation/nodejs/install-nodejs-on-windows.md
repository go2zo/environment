# Install NodeJS on Windows

## Using chocolatey

### `choco` 설치

- [`choco` 설치](/installation/chocolatey.md#install) 참조

### Node JS LTS 설치

[chocolatey에서 패키지를 검색](https://chocolatey.org/packages/nodejs-lts)하여 설치한다.

```powershell
> choco install nodejs-lts
```

### 설치 확인

```powershell
> node -v
v12.16.3
> npm -v
6.14.4
```

## Using installer

[NodeJS 다운로드](https://nodejs.org/ko/download/)에서 최신 Windows Installer를 다운로드 하여 설치한다.

>  chocolatey은 인스톨러가 제공되는 경우 기본적으로 인스톨러 설치가 된다.
>
> 따라서 설치 결과는 두 방법이 동일하다.

