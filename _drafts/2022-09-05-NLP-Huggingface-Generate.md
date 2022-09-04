---
title: '[NLP] 트랜스포머 (Transformer) - 모델 아키텍처를 중심으로'

categories:
  - Artificial Intelligence
tags:
  - Natural Language Processing

last_modified_at: 2022-09-05T08:06:00-05:00

classes: wide
use_math: true
---

이 글은 트랜스포머의 모델 아키텍처를 중점적으로 다룬 기록입니다.

## Transformer

트랜스포머(Transformer)는 어텐션(Attention)만으로 인코더-디코더(Encoder-Decoder) 구조를 설계한 모델입니다.



- `max_length`
  - `(int, optional, defaults to model.config.max_length)`
  - 생성되는 토큰의 최대 개수.
  - input prompt의 길이 + max_new_tokens 에 상응함.

- `max_new_tokens` `(int, optional)`
  - input prompt 내 토큰 개수를 제외하고, 생성되는 토큰의 최대 개수.

- `min_length` `(int, optional, default=10)`
  - 생성되는 토큰의 최소 개수

- `do_sample` `(bool, optional, default=False)`
  - Whether or not to use sampling ; use greedy decoding otherwise.
  - if True, 
  - if False, greedy decoding 사용

- `num_beams` `(int, optional, default=1)`
  - Number of beams for beam search. 1 means no beam search.

- `temperature` `(float, optional, default=1.0)`
  - The value used to module the next token probabilities.

- `top_k` `(int, optional, defaults to 50)`
  - The number of highest probability vocabulary tokens to keep for top-k-filtering.

- `top_p` `(float, optional, defaults to 1.0)`
  - If set to float < 1, only the most probable tokens with probabilities that add up to top_p or higher are kept for generation.

- `repetition_penalty` `(float, optional, default=1.0)`
  - repetition penalty를 위한 파라미터.
  - `1.0`은 no penalty를 의미함.

- `no_repeat_ngram_size` `(int, optional, defaults to 0)`
  - If set to int > 0, all ngrams of that size can only occur once.
  - `1`
  - `4` : 4-gram은 오직 한번만 등장할 수 있음.

- `encoder_no_repeat_ngram_size` `(int, optional, defaults to 0)`
  - If set to int > 0, all ngrams of that size that occur in the encoder_input_ids cannot occur in the decoder_input_ids.



##

greedy decoding 
by calling greedy_search() if num_beams=1 and do_sample=False.

multinomial sampling 
by calling sample() if num_beams=1 and do_sample=True.

beam-search decoding 
by calling beam_search() if num_beams>1 and do_sample=False.

beam-search multinomial sampling 
by calling beam_sample() if num_beams>1 and do_sample=True.

diverse beam-search decoding by calling group_beam_search(), if num_beams>1 and num_beam_groups>1.

constrained beam-search decoding by calling constrained_beam_search(), if constraints!=None or force_words_ids!=None.

## References


https://huggingface.co/docs/transformers/v4.21.2/en/main_classes/text_generation#transformers.generation_utils.GenerationMixin.generate

