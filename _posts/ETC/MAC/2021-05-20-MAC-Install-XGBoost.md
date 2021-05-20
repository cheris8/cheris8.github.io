---
title: '[macOS] XGBoost 설치'

categories:
  - Etc
tags:


last_modified_at: 2021-05-20T08:06:00-05:00

classes: wide
---

이 글은 macOS Catalina에서 XGBoost를 설치하는 방법에 관한 기록입니다.

```bash
kimchaehyeong@gimchaehyeong-ui-MacBookPro ~ % cd xgboost
kimchaehyeong@gimchaehyeong-ui-MacBookPro xgboost % mkdir build
kimchaehyeong@gimchaehyeong-ui-MacBookPro xgboost % cd build
kimchaehyeong@gimchaehyeong-ui-MacBookPro build % cmake ..
kimchaehyeong@gimchaehyeong-ui-MacBookPro build % make -j4
kimchaehyeong@gimchaehyeong-ui-MacBookPro build % cd ..
kimchaehyeong@gimchaehyeong-ui-MacBookPro xgboost % cd python-package
kimchaehyeong@gimchaehyeong-ui-MacBookPro python-package % sudo python setup.py install
```
