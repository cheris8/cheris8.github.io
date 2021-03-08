---
title: '[마크다운] Markdown을 PDF로 변환하는 방법'

categories:
  - Etc
tags:
  - Markdown

last_modified_at: 2021-02-04T08:06:00-05:00

classes: wide
---

이 글은 markdown을 pdf로 변환하는 방법에 관한 기록입니다.

## `Pandoc`

`pandoc`은 마크다운을 다양한 포맷으로 변환해주는 프로그램입니다. 맥OS에서는 아래의 명령어를 통해 설치할 수 있습니다.

```bash
# homebrew를 통해 pandoc 설치
brew install pandoc
```

`pandoc`에서 제공하는 옵션은 다음과 같습니다.

|옵션 (short)|옵션 (long)       |의미        |
|-----------|-----------------|-----------|
|-o FILENAME|--output=FILENAME|output 파일명|
|-f FORMAT  |--from=FORMAT    |input 파일 포맷|
|-t FORMAT  |--to=FORMAT      |output 파일 포맷|
|--toc      |                 |목차 생성|
|-S         |--smart pandoc   |input 포맷을 스스로 판단하여 처리|
|-s         |--standalone     |파일이 아닌 STDIN에서 입력 수행|
|-c URL     |--css=URL        |파일 변환 시 사용할 CSS의 URL|
|-H FILENAME|--include-in-header=FILENAME|FILENAME을 header로 사용|	
|-A FILENAME|--include-after-body=FILENAME|FILENAME을 footer로 사용	|

예를 들어, `MARKDOWN.md` 파일을 `PDF.pdf` 파일로 변환하는 방법은 다음과 같습니다.

```bash
pandoc MARKDOWN.md -f markdown -t pdf -o PDF.pdf
```

또, markdown 파일에 LaTex 수식이 포함되어 있는 경우 아래의 명령어를 통해 markdown 파일을 pdf 파일로 변환할 수 있습니다. 이때 폰트를 설정해주어야 합니다.

```bash
# 한국어 폰트 리스트를 확인
fc-list :lang=ko 

# MARKDOWN.md 파일을 PDF.pdf 파일로 변환
pandoc MARKDOWN.md -o PDF.pdf --pdf-engine=xelatex -V mainfont='Noto Sans CJK KR'
```

