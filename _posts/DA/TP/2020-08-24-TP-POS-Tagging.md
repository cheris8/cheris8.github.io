---
title: '[텍스트 전처리] 품사 태깅 (Part-Of-Speech Tagging ; POS Tagging)'

categories:
  - Data Analysis
  - Text Preprocessing
tags:
  - Data Analysis
  - Text Preprocessing
  - Text Mining
  - Natural Language Processing

last_modified_at: 2020-08-24T08:06:00-05:00

classes: wide
---

이 글은 품사 태깅에 관한 기록입니다.

## 품사 태깅 (Part-Of-Speech Tagging ; POS Tagging)

때때로 단어는 표기는 같지만 품사에 따라 단어의 의미가 달라지는 경우가 발생합니다.

예를 들어, 영어의 경우, 단어 "fly"는 동사로 쓰일 때에는 '날다'라는 의미를 갖는 한편 명사로 쓰일 때에는 '파리'라는 의미를 갖습니다. 한국어의 경우, "못"이라는 단어는 명사일 때에는 망치를 사용해서 목재 따위를 고정하는 물건이라는 의미로 쓰이는 한편 부사일 때에는 동작 동사를 할 수 없다는 의미로 쓰입니다.

결과적으로, 이는 단어의 의미를 제대로 파악하기 위해서는 해당 단어의 품사 정보가 필요하다는 것을 시사합니다. 이에 따라 단어 토큰화 과정에서 각 단어가 어떤 품사로 쓰였는지 구분해놓기도 하는데, 이를 품사 태깅(Part-Of-Speech tagging ; POS Tagging)이라고 합니다.

### Penn Treebank

|Number|Tag|Description|
|------|---|-----------|
|1.|CC |Coordinating conjunction|
|2.|CD |Cardinal number|
|3.|DT |Determiner|
|4.|EX |Existential there|
|5.|FW |Foreign word|
|6.|IN |Preposition or subordinating conjunction|
|7.|JJ |Adjective|
|8.|JJR|Adjective, comparative|
|9.|JJS|Adjective, superlative|
|10.|LS|List item marker|
|11.|MD|Modal|
|12.|NN|Noun, singular or mass|
|13.|NNS|Noun, plural|
|14.|NNP|Proper noun, singular|
|15.|NNPS|Proper noun, plural|
|16.|PDT|Predeterminer|
|17.|POS|Possessive ending|
|18.|PRP|Personal pronoun|
|19.|PRP$|Possessive pronoun|
|20.|RB |Adverb|
|21.|RBR|Adverb, comparative|
|22.|RBS|Adverb, superlative|
|23.|RP	|Particle|
|24.|SYM|Symbol|
|25.|TO	|to|
|26.|UH	|Interjection|
|27.|VB	|Verb, base form|
|28.|VBD|Verb, past tense|
|29.|VBG|Verb, gerund or present participle|
|30.|VBN|Verb, past participle|
|31.|VBP|Verb, non-3rd person singular present|
|32.|VBZ|Verb, 3rd person singular present|
|33.|WDT|Wh-determiner|
|34.|WP	|Wh-pronoun|
|35.|WP$|Possessive wh-pronoun|
|36.|WRB|Wh-adverb|

### 세종 품사 태그

|대분류|태그|설명|
|----|---|---|
|체언|NNG|일반 명사|
|체언|NNP|고유 명사|
|체언|NNB|의존 명사|
|체언|NR |수사|
|체언|NP |대명사|
|용언|VV |동사|
|용언|VA |형용사|
|용언|VX |보조 용언|
|용언|VCP|긍정 지정사|
|용언|VCN|부정 지정사|
|관형사|MM|관형사|
|부사|MAG|일반 부사|
|부사|MAJ|접속 부사|
|감탄사|IC|감탄사|
|조사|JKS|주격 조사|
|조사|JKC|보격 조사|
|조사|JKG|관형격 조사|
|조사|JKO|목적격 조사|
|조사|JKB|부사격 조사|
|조사|JKV|호격 조사|
|조사|JKQ|인용격 조사|
|조사|JX |보조사|
|조사|JC |접속 조사|
|선어말 어미|EP|선어말 어미|
|어말 어미|EF|종결 어미|
|어말 어미|EC|연결 어미|
|어말 어미|ETN|명사형 전성 어미|
|어말 어미|ETM|관형형 전성 어미|
|접두사|XPN|체언 접두사|
|접미사|XSN|명사 파생 접미사|
|접미사|XSV|동사 파생 접미사|
|접미사|XSA|형용사 파생 접미사|
|어근|XR|어근|
|부호|SF|마침표, 물음표, 느낌표|
|부호|SP|쉼표, 가운뎃점, 콜론, 빗금|
|부호|SS|따옴표, 괄호표, 줄표|
|부호|SE|줄임표|
|부호|SO|붙임표 (물결, 숨김, 빠짐)|
|부호|SW|기타기호 (논리수학기호, 화폐기호)|
|분석 불능|NF|명사추정범주|
|분석 불능|NV|용언추정범주|
|분석 불능|NA|분석불능범주|
|한글 이외|SL|외국어|
|한글 이외|SH|한자|
|한글 이외|SN|숫자|

