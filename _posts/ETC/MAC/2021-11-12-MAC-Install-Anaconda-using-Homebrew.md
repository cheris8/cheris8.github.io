---
title: '[macOS] Homebrew를 이용하여 Anaconda 설치'

categories:
  - Etc
tags:


last_modified_at: 2021-11-12T08:06:00-05:00

classes: wide
---

이 글은 macOS에서 Homebrew를 이용하여 Anaconda를 설치하는 방법에 관한 기록입니다.

```bash
brew install anaconda
```

```bash
vim ~/.zshrc
export PATH="/usr/local/anaconda3/bin:$PATH"
source ~/.zshrc
```

```bash
/opt/homebrew/anaconda3/bin/conda init zsh
```
