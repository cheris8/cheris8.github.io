---
title: '[macOS] 서버 관련 명령어'

categories:
  - Etc
tags:

last_modified_at: 2021-11-03T08:06:00-05:00

classes: wide
---

이 글은 서버와 관련된 명령어들을 정리한 기록입니다.

```bash
# cuda 버전 확인
nvcc -V

# cuda 버전 확인
nvidia-smi
```

### From Local To Server

```bash
# 로컬에서 서버(11번)으로 폴더 보내기
scp -r /Users/kimchaehyeong/Downloads/data kimchaehyeong@XX.XX.11:/home/kimchaehyeong/

# 로컬에서 서버(11번)으로 파일 보내기
scp -r /Users/kimchaehyeong/Downloads/train.csv kimchaehyeong@XX.XX.11:/home/kimchaehyeong/data/
```

### From Server To Server

```bash
# 서버(11번)에서 서버(22번)으로 폴더 보내기
scp -r kimchaehyeong@XX.XX.11:/home/kimchaehyeong/data kimchaehyeong@XX.XX.22:/home/kimchaehyeong/

# 서버(11번)에서 서버(22번)으로 파일 보내기
scp kimchaehyeong@XX.XX.11:/home/kimchaehyeong/Downloads/train.csv kimchaehyeong@XX.XX.22:/home/kimchaehyeong/data/
```