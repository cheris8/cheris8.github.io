---
title: '[마크다운] Jupyter notebook을 Markdown으로 변환하는 방법'

categories:
  - ETC
tags:
  - Markdown

last_modified_at: 2021-02-03T08:06:00-05:00

classes: wide
---

이 글은 jupyter notebook을 markdown으로 변환하는 방법에 관한 기록입니다. 궁극적으로는 이를 활용하여 GitHub 블로그에 jupyter notebook을 포스팅으로 업로드 하고자 합니다.

## `nbconvert`

`nbconvert`은 jupyter notebook 파일을 markdown 파일 혹은 html 파일로 변환해주는 프로그램입니다. 아래의 명령어를 통해 설치할 수 있습니다.

```bash
# pip을 통해 nbconvert 설치
pip install nbconvert
```

```bash
# conda를 통해 nbconvert 설치
conda install nbconvert
```

예를 들어, `JUPYTER_NOTEBOOK.ipynb`라는 jupyter notebook 파일을 markdown 파일로 변환하고 싶다고 가정해봅시다. 이를 위해 먼저 `JUPYTER_NOTEBOOK.ipynb` 파일이 있는 경로로 이동합니다. 그러고 나서 아래의 명령어를 실행합니다.

```bash
jupyter nbconvert --to markdown JUPYTER_NOTEBOOK.ipynb
```

위의 명령어는 해당 경로에 다음의 두가지 결과물을 생성합니다.

- `JUPYTER_NOTEBOOK.md` : markdown 파일
- `JUPYTER_NOTEBOOK_files` : markdown 파일에 포함되어 있는 모든 이미지들을 모아놓은 폴더

이를 활용하여 GitHub 블로그에 포스팅 하기 위해서는 다음과 같은 작업을 거쳐야 합니다.

1. `JUPYTER_NOTEBOOK.md` 파일 내 상단에 YAML 부분을 추가합니다.
2. `JUPYTER_NOTEBOOK.md` 파일 내 이미지들의 경로를 블로그의 이미지 경로 형태로 수정합니다.
3. `JUPYTER_NOTEBOOK.md` 파일의 파일명을 날짜(date)와 제목(title)을 포함하는 `YYYY-MM-DD-TITLE.md` 형태로 바꾸어줍니다.
2. 이 `YYYY-MM-DD-TITLE.md` 파일을 블로그의 `_posts` 폴더로 이동시킵니다.
5. `JUPYTER_NOTEBOOK_files` 폴더를 블로그의 `images` 폴더로 이동시킵니다.

이제 이 파일들을 모두 add/commit/push하면 GitHub 블로그에 포스팅이 추가되는 것을 확인할 수 있습니다.

