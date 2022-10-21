---
title: '[macOS] 깃허브 블로그를 위한 Homebrew 및 Ruby 설치'

categories:
  - Etc
tags:

last_modified_at: 2021-11-11T08:06:00-05:00

classes: wide
use_math: true
---

이 글은 깃허브 블로그를 만들고자 Homebrew와 Ruby를 설치하는 방법에 관하여 정리한 기록입니다.

### 1. Homebrew 설치

[https://brew.sh](https://brew.sh)에 접속하여 설치 명령어를 확인합니다.

```bash
# homebrew 설치
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

`.zprofile` 파일에 homebrew PATH를 추가 및 적용합니다.

```bash
# ==> Next steps:
# - Run these three commands in your terminal to add Homebrew to your PATH:
echo '# Set PATH, MANPATH, etc., for Homebrew.' >> /Users/kimchaehyeong/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/kimchaehyeong/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

```bash
# homebrew 버전 확인
brew --version
```
```
Homebrew 3.6.6
Homebrew/homebrew-core (git revision 02196965da4; last commit 2022-10-21)
```

### 2. Ruby 설치

macOS에는 기본적으로 ruby가 설치되어 있습니다.

```bash
# ruby 버전 확인
ruby -v
```
```
ruby 2.6.8p205 (2021-07-07 revision 67951) [universal.arm64e-darwin21]
```

이를 rbenv로 관리되는 ruby로 바꿔주어야 합니다. 이를 위해 먼저 rbenv를 설치합니다.

```bash
# rbenv 설치
brew install rbenv ruby-build
```

```bash
# rbenv 버전 확인
rbenv versions
```
```
* system
```

다음으로 ruby를 설치합니다.

```bash
# 설치 가능한 ruby 목록 확인
rbenv install -l
```
```
2.6.10
2.7.6
3.0.4
3.1.2
jruby-9.3.8.0
mruby-3.1.0
picoruby-3.0.0
rbx-5.0
truffleruby-22.2.0
truffleruby+graalvm-22.2.0
```

```bash
# ruby 설치
rbenv install 3.1.2
```

```bash
# ruby 버전 확인
rbenv versions 
```
```
* system
  3.1.2
```

```bash
# ruby 3.1.2 버전을 글로벌로 변경
rbenv global 3.1.2
# ruby 버전 확인
rbenv versions
```
```
  system
* 3.1.2 (set by /Users/kimchaehyeong/.rbenv/version)
```

`.zshrc` 파일에 rbenv PATH를 추가 및 적용합니다.

```bash
vim ~/.zshrc
```
```
[[ -d ~/.rbenv  ]] && \
  export PATH=${HOME}/.rbenv/bin:${PATH} && \
  eval "$(rbenv init -)"
```
```bash
source ~/.zshrc
```

### 3. Bundler 설치

```bash
gem install bundler
```

### 4. Jekyll 설치

```bash
gem install jekyll
```

### 5. 깃허브 블로그를 웹사이트에 호스팅

```bash
bundle exec jekyll serve
```

하지만 아래와 같은 에러가 발생합니다.

```
Calling `DidYouMean::SPELL_CHECKERS.merge!(error_name => spell_checker)' has been deprecated. Please call `DidYouMean.correct_error(error_name, spell_checker)' instead.
Unable to find a spec satisfying nokogiri (>= 1.10.4, < 2.0) in the set. Perhaps the lockfile is corrupted? Found nokogiri (1.11.1-x86_64-darwin) that did not match the current platform.
Run `bundle install` to install missing gems.
```

시키는 대로 아래와 같은 명령어를 실행합니다.

```bash
bundle install
```

하지만 아래와 같은 에러가 발생합니다.

```
Running 'configure' for libxml2 2.9.10... ERROR, review

...중략...

An error occurred while installing nokogiri (1.11.1), and Bundler cannot continue.
Make sure that `gem install nokogiri -v '1.11.1' --source 'https://rubygems.org/'` succeeds
before bundling.

In Gemfile:
  jekyll-algolia was resolved to 1.6.0, which depends on
    algolia_html_extractor was resolved to 2.6.4, which depends on
      nokogiri
```

아래와 같은 명령어를 실행합니다.

```bash
gem install nokogiri --platform=ruby
```

```bash
bundle install
```

```bash
bundle exec jekyll serve
```

[http://127.0.0.1:4000](http://127.0.0.1:4000)에 접속해보면 정상적으로 호스팅 되는 것을 확인할 수 있습니다.
