---
title: '[NLP] 허깅페이스 (HuggingFace) transformers를 활용한 텍스트 생성 (Text Generation) 방법'

categories:
  - Artificial Intelligence
tags:
  - Natural Language Processing

last_modified_at: 2023-02-09T08:06:00-05:00

classes: wide
use_math: true
---

이 글은 HuggingFace 내 `transformers` 라이브러리를 활용하여 텍스트를 생성하는 방법에 관한 기록입니다.

## Decoding strategies

`transformers` 라이브러리는 `model.generate()` 메소드를 제공합니다. 이를 활용하여 다음과 같은 다양한 디코딩 방법을 간편하게 구현할 수 있습니다.

- **greedy decoding**
    - arguments : `do_sample=False` and `num_beams=1`
    - `model.generate(do_sample=False, num_beams=1)`
- **multinomial sampling**
    - arguments : `do_sample=True` and `num_beams=1`
    - `model.generate(do_sample=True, num_beams=1)`
- **beam-search decoding**
    - arguments : `do_sample=False` and `num_beams>1`
    - `model.generate(do_sample=False, num_beams>1)`
- **beam-search multinomial sampling**
    - arguments : `do_sample=True` and `num_beams>1`
    - `model.generate(do_sample=True, num_beams>1)`
- **diverse beam-search decoding**
    - arguments : `num_beams>1` and `num_beam_groups>1`
    - `model.generate(num_beams>1, num_beam_groups>1)`
- **contrastive search**
    - arguments : `penalty_alpha>0.` and `top_k>1`
    - `model.generate(penalty_alpha>0, top_k>1)`
- **constrained beam-search decoding**
    - arguments : `constraints!=None` or `force_words_ids!=None`

## Arguments

`model.generate()` 메소드에서 사용 가능한 arguments는 아래와 같습니다.

- **max_length** (type : `int`, optional, defaults to `20`)
    - 생성할 token들의 최대 개수 (input prompt 내 token들의 개수 포함)
- **max_new_tokens** (type : `int`, optional)
    - 생성할 token들의 최대 개수 (input prompt 내 token들의 개수 제외)
- **min_length** (type : `int`, optional, defaults to `0`)
    - 생성할 token들의 최소 개수 (input prompt 내 token들의 개수 포함)
- **min_new_tokens** (type : `int`, optional)
    - 생성되는 token들의 최소 길이 (input prompt 내 token들의 개수 제외)
- **do_sample** (`bool`, optional, defaults to `False`)
    - sampling의 사용 여부
        - `do_sample=False` 는 greedy decoding을 의미함.
        - `do_sample=True` 는 multi-nomial sampling을 의미함.
- **num_beams** (`int`, optional, defaults to `1`)
    - beam search를 위한 beams의 개수
        - `num_beams=1` 은 beam search를 하지 않는 것을 의미함.
- **num_beam_groups** (`int`, optional, defaults to `1`)
    - 서로 다른 그룹 간의 다양성을 보장하기 위해 `num_beams` 를 분할할 group의 수
- **temperature** (`float`, optional, defaults to `1.0`)
    - 다음 토큰의 확률을 모듈화 하는 데에 사용되는 값
        - `temperature>0.5` 는 값이 커질수록 다양한 text을 생성함.
- **top_k** (`int`, optional, defaults to `50`)
    - top-k sampling에서 k 값
    - top-k sampling을 위한 파라미터
    - top-k-filtering을 위해 보관할 가장 높은 확률을 갖는 vocabulary token의 수
- **top_p** (`float`, optional, defaults to `1.0`)
    - top-p sampling에서 p 값
    - top-p sampling을 위한 파라미터
        - `top_p<1.0` 은 해당 값 이상의 확률을 가진 가장 가능성 있는 token들의 가장 작은 집합만이 생성하는 동안 유지됨을 의미함.
- **repetition_penalty** (`float`, optional, defaults to `1.0`)
    - repetition penalty 값
    - repetition penalty에 대한 파라미터
        - `repetition_penalty=1.0` 은 no penalty를 의미함.
- **encoder_repetition_penalty** (`float`, optional, defaults to `1.0`)
    - original input에 없는 sequence에 대한 exponential penalty 값
        - `encoder_repetition_penalty=1.0` 은 no penalty를 의미함.
- **length_penalty** (`float`, *optional*, defaults to `1.0`)
    - 생성되는 sequence의 길이에 대한 exponential penalty 값 (beam 기반 생성과 함께 사용)
    - `length_penalty>0.0` 은 더 긴 sequence를 생성하도록 함.
    - `length_penalty<0.0` 은 더 짧은 sequence를 생성하도록 함.
- **no_repeat_ngram_size** (`int`, optional, defaults to `0`)
    - `no_repeat_ngram_size>0` 은 해당 크기의 ngram들이 오직 한번만 발생할 수 있음을 의미함.
- **forced_bos_token_id** (`int`, optional, defaults to `model.config.forced_bos_token_id`)
    - `decoder_start_token_id` 다음에 처음으로 생성할 token으로 강제할 token의 `token_id`
- **forced_eos_token_id** (`Union[int, List[int]]`, optional, defaults to `model.config.forced_eos_token_id`)
    - The id of the token to force as the last generated token when `max_length` is reached. Optionally, use a list to set multiple *end-of-sequence* tokens.
- **forced_decoder_ids** (`List[List[int]]`, optional)
    - sampling 전에 강제로 적용되는, generation index로부터 token index로의 mapping을 나타내는 integer pair의 list
        - 예를 들어, `[[1,123]]` 은 두번째로 생성된 token이 항상 index가 123인 token이 되도록 한다는 것을 의미함.
- **num_return_sequences** (`int`, optional, defaults to `1`)
    - 배치 내 각 element에 대하여 독립적으로 계산하여 반환될 sequence의 수
- **pad_token_id** (`int`, optional)
    - The id of the *padding* token.
- **bos_token_id** (`int`, optional)
    - The id of the *beginning-of-sequence* token.
- **eos_token_id** (`Union[int, List[int]]`, optional)
    - The id of the *end-of-sequence* token. Optionally, use a list to set multiple *end-of-sequence* tokens.
- **encoder_no_repeat_ngram_size** (`int`, optional, defaults to `0`)
    - `encoder_no_repeat_ngram_size>0` 은 `encoder_input_ids` 에서 발생한 해당 크기의 모든 ngrams는 `decoder_input_ids` 에서 발생할 수 없음을 의미함.
- **decoder_start_token_id** (`int`, optional)
    - encoder-decoder 모델이 `bos_token` 말고 다른 token으로 decoding을 시작하는 경우 해당 token의 `token_id`

## References

[How to generate text: using different decoding methods for language generation with Transformers](https://huggingface.co/blog/how-to-generate?fbclid=IwAR2BZ4BNG0PbOvS5QaPLE0L3lx7_GOy_ePVu4X1LyTktQo-nLEPr7eht1O0)


[Text Generation](https://huggingface.co/docs/transformers/v4.26.1/en/main_classes/text_generation)
