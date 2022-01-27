---
title: '[macOS] 서버 관련 명령어'

categories:
  - Etc
tags:

last_modified_at: 2021-11-03T08:06:00-05:00

classes: wide
---

이 글은 서버와 관련된 명령어들을 정리한 기록입니다.


구문 : scp [옵션] [디렉터리 이름] [원격지_id]@[원격지_ip]:[보낼 경로]

```bash

# cuda 버전 확인
nvcc -V

#
nvidia-smi


scp yen@165.132.144.84:/data/yen/1008_severity_movie_code/new_dataset_past/new_movie_data ./

# 로컬에서 서버()로 폴더 보내기

scp -r [[LOCAL PATH]] [[USER NAME]]@[[HOST]]:[[SERVER PATH]]



```


### From Local To Server

```bash


# 로컬에서 서버(228번)으로 폴더 보내기
scp -r /Users/kimchaehyeong/Downloads/VQG-2020-TrainSet chaehyeong@165.132.142.228:/home/chaehyeong/VQA-Med


# 로컬에서 서버(23번)으로 폴더 보내기
scp -r /Users/kimchaehyeong/Documents/LREC_Code yen@165.132.144.23:/data/yen

scp -r /Users/kimchaehyeong/Documents/Deliver_dataset chaehyeong@165.132.144.84:/home/chaehyeong/MARS/LREC_Code

# 로컬에서 서버(84번)으로 폴더 보내기
scp -r /Users/kimchaehyeong/Documents/CAES/Long-document-dataset chaehyeong@165.132.144.84:/home/chaehyeong/CAES



scp -r LREC_Code yen@165.132.144.24:/data/yen



scp -r /data/yen/LREC_Code yen@165.132.144.24:/data/yen

scp -r /Users/kimchaehyeong/Downloads/data chaehyeong@165.132.142.228:/home/chaehyeong/CAES/



scp -r chaehyeong@165.132.144.84:/home/chaehyeong/CAES chaehyeong@165.132.142.228:/home/chaehyeong/CAES/



scp -r /Users/kimchaehyeong/Downloads/long_text chaehyeong@165.132.144.84:/home/chaehyeong/CAES/


# 서버에서 로컬()로 파일 보내기
scp yen@165.132.144.84:/home/yen/1029_d2_asp_rat/anal/counterfactual/plot.ipynb /Users/kimchaehyeong/Documents/MARS/



# 로컬에서 서버로 파일 보내기
scp /Users/kimchaehyeong/Documents/MARS/plot.ipynb chaehyeong@165.132.144.84:/home/chaehyeong/MARS





```

### From Server To Server


```bash
# 서버(84번)에서 서버(84번)으로 파일 보내기
scp yen@165.132.144.84:/home/yen/MARS/data.csv chaehyeong@165.132.144.84:/home/chaehyeong/MARS/

# 서버(140번)에서 서버(84번)으로 폴더 보내기
scp -r minju@165.132.141.140:/home/minju/long_text chaehyeong@165.132.144.84:/home/chaehyeong/CAES


# 서버(84번)에서 서버(23번)으로 폴더 보내기
scp -r chaehyeong@165.132.144.84:/home/chaehyeong/CAES/data chaehyeong@165.132.141.140:/home/chaehyeong/CAES/



scp -r chaehyeong@165.132.144.84:/home/chaehyeong/CAES/data yen@165.132.144.23:/data/yen



```