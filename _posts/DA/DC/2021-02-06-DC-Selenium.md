---
title: '[데이터 크롤링] Selenium with Python'

categories:
  - Data Analysis
tags:
  - Data Analysis
  - Data Crawling

last_modified_at: 2021-02-06T08:06:00-05:00

classes: wide
---

이 글은 파이썬에서 셀레니움(Selenium)을 사용하는 방법에 관한 기록입니다.

## 1. Import Library

```python
from selenium import webdriver

from selenium.webdriver import ActionChains

from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By

from selenium.webdriver.support.ui import Select
from selenium.webdriver.support.ui import WebDriverWait
```

## 2. Settings

```python
from selenium import webdriver

path = 'chromedriver가 존재하는 경로'
url = '크롤링 하고자 하는 웹사이트 url'

# 브라우저 생성
driver = webdriver.Chrome(path)

# 브라우저에 url 호출
driver.get(url)

# 현재 url 반환
driver.current_url

# 브라우저 종료
drver.quit()
```

## 3. Locating Elements : 요소 선택

`find_element_by_XXXX` 메소드는 조건과 일치하는 요소 한 개를, `find_elements_by_XXXX` 메소드는 조건과 일치하는 요소 여러 개를 선택합니다. 

- `driver.find_element_by_id` : `id` 속성으로 일치하는 요소 선택하기
- `driver.find_element_by_name` : `name` 속성으로 일치하는 요소 선택하기
- `driver.find_element_by_tag_name` : 태그명으로 일치하는 요소 선택하기
- `driver.find_element_by_class_name` : 클래스명으로 일치하는 요소 선택하기
- `driver.find_element_by_xpath` : XPath로 일치하는 요소 선택하기
- `driver.find_element_by_css_selector` : CSS Selector로 일치하는 요소 선택하기
- `driver.find_element_by_link_text` : 태그 `<a>` 내 `href` 속성으로 일치하는 요소 선택하기
- `driver.find_element_by_partial_link_text` : 태그 `<a>` 내 `href` 속성으로 일치하는 요소 선택하기

- `driver.find_elements_by_name` : `name` 속성으로 일치하는 모든 요소 선택하기
- `driver.find_elements_by_tag_name` : 태그명으로 일치하는 모든 요소 선택하기
- `driver.find_elements_by_class_name` : 클래스명으로 일치하는 모든 요소 선택하기
- `driver.find_elements_by_xpath` : XPath로 일치하는 모든 요소 선택하기
- `driver.find_elements_by_css_selector` : CSS Selector로 일치하는 모든 요소 선택하기
- `driver.find_elements_by_link_text` : 태그 `<a>` 내 `href` 속성으로 일치하는 모든 요소 선택하기
- `driver.find_elements_by_partial_link_text` : 태그 `<a>` 내 `href` 속성으로 일치하는 모든 요소 선택하기

## 4. Interacting with the page : 상호작용

### 키보드 입력

키보드를 입력하고자 할 때에는 `send_keys()` 메소드를 사용할 수 있습니다. `send_keys()` 메소드는 문자열 또는 `Keys` 모듈을 활용한 것을 입력으로 받습니다.

```python
from selenium.webdriver.common.keys import Keys

element.send_keys('QUERY')
element.send_keys(Keys.RETURN)
```

`Keys` 모듈은 키보드 자판에서 텍스트 이외의 특수문자를 입력할 수 있도록 지원합니다. 사용할 수 있는 키보드 자판 특수문자는 다음과 같습니다.

- Keys.NULL = '\ue000'
- Keys.CANCEL = '\ue001'  # ^break
- Keys.HELP = '\ue002'
- Keys.BACKSPACE = '\ue003'
- Keys.BACK_SPACE = BACKSPACE
- Keys.TAB = '\ue004'
- Keys.CLEAR = '\ue005'
- Keys.RETURN = '\ue006'
- Keys.ENTER = '\ue007'
- Keys.SHIFT = '\ue008'
- Keys.LEFT_SHIFT = SHIFT
- Keys.CONTROL = '\ue009'
- Keys.LEFT_CONTROL = CONTROL
- Keys.ALT = '\ue00a'
- Keys.LEFT_ALT = ALT
- Keys.PAUSE = '\ue00b'
- Keys.ESCAPE = '\ue00c'
- Keys.SPACE = '\ue00d'
- Keys.PAGE_UP = '\ue00e'
- Keys.PAGE_DOWN = '\ue00f'
- Keys.END = '\ue010'
- Keys.HOME = '\ue011'
- Keys.LEFT = '\ue012'
- Keys.ARROW_LEFT = LEFT
- Keys.UP = '\ue013'
- Keys.ARROW_UP = UP
- Keys.RIGHT = '\ue014'
- Keys.ARROW_RIGHT = RIGHT
- Keys.DOWN = '\ue015'
- Keys.ARROW_DOWN = DOWN
- Keys.INSERT = '\ue016'
- Keys.DELETE = '\ue017'
- Keys.SEMICOLON = '\ue018'
- Keys.EQUALS = '\ue019'

- Keys.NUMPAD0 = '\ue01a'  
- Keys.NUMPAD1 = '\ue01b'
- Keys.NUMPAD2 = '\ue01c'
- Keys.NUMPAD3 = '\ue01d'
- Keys.NUMPAD4 = '\ue01e'
- Keys.NUMPAD5 = '\ue01f'
- Keys.NUMPAD6 = '\ue020'
- Keys.NUMPAD7 = '\ue021'
- Keys.NUMPAD8 = '\ue022'
- Keys.NUMPAD9 = '\ue023'
- Keys.MULTIPLY = '\ue024'
- Keys.ADD = '\ue025'
- Keys.SEPARATOR = '\ue026'
- Keys.SUBTRACT = '\ue027'
- Keys.DECIMAL = '\ue028'
- Keys.DIVIDE = '\ue029'

- Keys.F1 = '\ue031'  
- Keys.F2 = '\ue032'
- Keys.F3 = '\ue033'
- Keys.F4 = '\ue034'
- Keys.F5 = '\ue035'
- Keys.F6 = '\ue036'
- Keys.F7 = '\ue037'
- Keys.F8 = '\ue038'
- Keys.F9 = '\ue039'
- Keys.F10 = '\ue03a'
- Keys.F11 = '\ue03b'
- Keys.F12 = '\ue03c'

- Keys.META = '\ue03d'
- Keys.COMMAND = '\ue03d'

입력한 것을 지우고자 할 때에는 `clear()` 메소드를 사용할 수 있습니다.

```python
element.clear()
```

### Click

어떠한 요소를 클릭하고자 할 때에는 `click()` 메소드를 사용합니다.

```python
element.click()
```

### Select box

`Select`를 통해 select box를 다룰 수 있습니다.

```python
from selenium.webdriver.support.ui import Select

selectbox = Select(driver.find_element_by_xpath('SELECT_BOX_XPATH'))

# 선택하기
selectbox.select_by_index('INDEX')
selectbox.select_by_value('VALUE')
selectbox.select_by_visible_text('TEXT')

# 선택해제하기
selectbox.deselect_by_index('INDEX')
selectbox.deselect_by_value('VALUE')
selectbox.deselect_by_visible_text('TEXT')
selectbox.deselect_all()
```

### Drag and Drop

`ActionChains` 모듈의 `drag_and_drop` 메소드는 source 요소에서 target 요소로 drag 및 drop을 할 수 있도록 합니다.

```python
from selenium.webdriver import ActionChains

source = driver.find_element_by_name('SOURCE')
target = driver.find_element_by_name('TARGET')

action_chains = ActionChains(driver)
action_chains.drag_and_drop(source, target).perform()
```

## 5. Handling Webpage

웹페이지에서 가능한 다음 페이지로 이동, 이전 페이지로 이동, 웹페이지 내 스크롤 등의 작업을 수행할 수 있습니다.

- `driver.forward()` : 다음 페이지로 이동하기 (앞으로 가기)
- `driver.back()` : 이전 페이지로 이동하기 (뒤로 가기)
- `driver.execute_script('window.scrollTo(0, document.body.scrollHeight);')` : 웹페이지 내 최하단까지 스크롤 하여 맨 밑으로 내려가기
- `ActionChains(driver).move_to_element(element).perform()` : 웹페이지 내 `element`까지 스크롤 하여 특정 요소까지 내려가기

## 6. Chrome Options

브라우저에 관하여 여러가지 옵션을 설정할 수 있습니다. 사용할 수 있는 옵션들은 다음과 같습니다.

```python
# 옵션 생성
options = webdriver.ChromeOptions()

# 옵션 추가
options.add_argument('headless') # 크롬 브라우저를 띄우지 않도록 설정
options.add_argument('window-size=1920x1080') # 브라우저 창 크기 설정
options.add_argument('lang=ko_KR') # 언어 설정
options.add_argument('disable-gpu') # GPU를 사용하지 않도록 설정
options.add_argument('disable-infobars')
options.add_argument('--disable-extensions')
options.add_experimental_option('excludeSwitches', ['enable-automation'])
options.add_experimental_option('useAutomationExtension', False)
options.add_argument('--disable-blink-features=AutomationControlled')
options.add_experimental_option('debuggerAddress', '127.0.0.1:9222')

# 브라우저 옵션을 적용하여 드라이버 생성
driver = webdriver.Chrome(path, options=options) 
```

## 7. 참고자료

[Selenium with Python](https://selenium-python.readthedocs.io)
