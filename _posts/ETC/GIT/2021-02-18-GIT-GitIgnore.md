---
title: '[Git] .gitignore 사용하기'

categories:
  - Etc
tags:
  - Git

last_modified_at: 2021-02-18T08:06:00-05:00

classes: wide
---

이 글은 `.gitignore`의 개념 및 사용 방법에 관한 기록입니다.

## `.gitignore` 개념

`.gitignore`는 특정 파일 혹은 디렉토리를 깃에서 관리하지 않도록 제외하는 역할을 합니다.

## `.gitignore` 사용 방법

먼저 로컬 PC에서 깃 저장소인 최상위 경로로 이동합니다.

```bash
cd Documents/DataScience
```

`.gitignore`를 생성합니다.

```bash
touch .gitignore
```

`.gitignore`를 엽니다.

```bash
vim .gitignore
```

vim 창이 열리면 `i`를 입력하여 INSERT 모드로 변경합니다. 그러고 나서 제외하고 싶은 파일/폴더 목록을 문법에 맞춰 작성해줍니다. 이를 자동으로 생성해주는 [웹사이트](https://www.toptal.com/developers/gitignore)를 이용하면 편리합니다. 작성이 끝나면, `esc`키를 누른 뒤 `:wq`를 입력하여 vim 창을 닫습니다.

이제 깃에 올리면 끝!

```bash
git add .
git commit -m 'upload .gitignore'
git push origin master
```

### `.gitignore`가 적용이 안 되는 경우

```bash
git rm -r --cached .
git add .
git commit -m "fixed untracked files"
```

