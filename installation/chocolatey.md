# Chocolatey

## Requirements

- Windows 7+ / Windows Server 2003+
- PowerShell v2+
- .NET Framework 4+

## Installation {#installation}

1. `Windows + X`를 눌러 **Windows Powershell(관리자)** 실행

   - `Get-ExecutionPolicy`로 현재 실행정책 확인
   
   ```powershell
   > Get-ExecutionPolicy
   Retricted
   ```
   
   - `Retricted`인 경우 다른 정책으로 변경 필요
   
   ```powershell
   > Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
   ```

2. 아래 스크립트 실행 (수초 소요)

```powershell
> Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

3. `choco` 명령어로 확인

```powershell
> choco -V
Chocolatey v0.10.15
```


