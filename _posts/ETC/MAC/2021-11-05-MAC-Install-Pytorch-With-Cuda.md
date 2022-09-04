---
title: '[macOS] CUDA를 사용할 수 있도록 PyTorch 설치'

categories:
  - Etc
tags:

last_modified_at: 2021-11-05T08:06:00-05:00

classes: wide
---

이 글은 macOS에서 CUDA를 사용할 수 있도록 PyTorch를 설치하는 방법에 대한 기록입니다.

```bash
# cuda 버전 확인
nvcc -V
# 11.2
```

```bash
# cuda 버전 확인
nvidia-smi
# 11.4
```

```bash
# Wheel OSX
pip install torch==1.7.1+cu110 torchvision==0.8.2+cu110 torchaudio==0.7.2 -f https://download.pytorch.org/whl/torch_stable.html
```

## References

[Previous PyTorch Versions](https://pytorch.org/get-started/previous-versions/)
