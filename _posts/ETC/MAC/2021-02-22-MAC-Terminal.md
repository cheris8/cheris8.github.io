---
title: '[macOS] 터미널 명령어'

categories:
  - Etc
tags:

last_modified_at: 2021-02-22T08:06:00-05:00

classes: wide
---

이 글은 터미널에서 자주 사용하는 명령어들을 정리한 기록입니다.

## Bash

```bash
# 현재 경로에 존재하는 폴더 및 파일 목록 출력
ls

# 경로 이동
cd FOLDER_NAME/FILE_NAME

# 폴더 생성
mkdir FOLDER_NAME

# 폴더 삭제
rmdir FOLDER_NAME

# 파일 삭제
rm FILE_NAME

# 문자열 출력
echo STRING
```

```bash
# 파이썬 버전 확인
python —version
```

```bash
# pip으로 설치한 패키지 목록 출력
pip freeze

# 패키지 상세 정보 출력
pip show PACKAGE_NAME
```

## Homebrew

```bash
# brew를 최신 버전으로 업데이트
brew update

# brew에서 해당 패키지를 설치할 수 있도록 제공하는지 검색
brew search PACKAGE_NAME

# brew로 설치한 패키지 목록 출력
brew list

# 패키지 상세 정보 출력
brew info PACKAGE_NAME

# 패키지 설치
brew install PACKAGE_NAME
brew install PACKAGE_NAME==PACKAGE_VERSION

# 패키지 업데이트
brew upgrade
brew upgrade PACKAGE_NAME

# 패키지 삭제
brew uninstall PACKAGE_NAME
```

## Conda

```bash
# 아나콘다에 패키지 설치
conda install -c PACKAGE_NAME

# 아나콘다 가상환경 목록 출력
conda info --envs

# 아나콘다 가상환경 활성화
conda activate VIRTUAL_ENV_NAME

# 아나콘다 가상환경 종료
conda deactivate

# 아나콘다 가상환경에 패키지 설치
conda install -n VIRTUAL_ENV_NAME PACKAGE_NAME
```

## Vim(Vi) Editor

- 입력모드 : 실제로 텍스트를 입력할 수 있는 상태
    - `i`
    - `a`
    - `o`
    - `insert`
- 명령모드 : `esc`를 누른 상태
    - `x` : 커서 기준 한문자 삭제
    - `(n)dd` : 한줄 삭제 (숫자 n을 붙이면 커서 기준 아래쪽 n줄 삭제)
    - `(n)yy` : 한줄 복사 (숫자 n을 붙이면 커서 기준 아래쪽 n줄 복사)
    - `p` : 커서 기준 오른쪽에 붙여넣기
- 편집모드 : `esc`를 누른 상태
    - `:wq` : 작성 파일을 저장하고 종료
    - `:q!` : 작성 파일을 저장하지 않고 종료
    - `:set nu` : 줄번호 표시
    - `set nonu` : 줄번호 표시 해제

### `.zshrc` 수정하기

```bash
# .zshrc 열기
vi ~/.zshrc

# .zshrc 내용 수정하기

# 수정한 .zshrc 적용하기
source ~/.zshrc
exec $SHELL
```

