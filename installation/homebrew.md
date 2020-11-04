# Homebrew



## Trouble-shooting

### `brew update` error

오랜기간 brew 업데이트를 하지 않다보니 오랜만에 업데이트 하며 에러가 발생했다.

```bash
$ brew update
remote: Repository not found.
fatal: repository 'https://github.com/Homebrew/homebrew-dupes/' not found
```

실제로 brew 버전은 1.6대에 머물러 있었다.

`brew untap` 명령으로 에러나는 repository를 제거하고 `brew docker`, `brew cleanup` 으로 에러를 제거한다.

```bash
$ brew --version
Homebrew 1.6.2
homebrew/dupes (git revision 648a7c; last commit 2018-04-24)
$ brew untap homebrew/dupes
```



