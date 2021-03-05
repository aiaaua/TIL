## datetime

#### 날짜/시간 format

| format | explain              | example                                  |
| ------ | -------------------- | ---------------------------------------- |
| %a     | 영어 요일(축약)      | Sun, Mon, Tue, ... , Sat                 |
| %A     | 영어 요일            | Sunday, Monday, Tuesday, ... , Saturday  |
| %b     | 영어 월(축약)        | Jan, Feb, Mar, ... , Dec                 |
| %B     | 영어 월              | January, February, March, ... , December |
| %d     | 두 자리 일           | 01, 02, 03, ... , 31                     |
| %m     | 두 자리 월           | 01, 02, 03, ... ,12                      |
| %y     | 두 자리 연도         | 01, 02, 03, ... , 99                     |
| %Y     | 네 자리 연도         | 0001, 0002, 0003, ... , 9999             |
| %S     | 두 자리 초           | 00, 01, 02, ... , 59                     |
| %M     | 두 자리 분           | 00, 01, 02, ... , 59                     |
| %H     | 두 자리 시간(24시간) | 00, 01, 02, ... , 23                     |
| %I     | 두 자리 시간(12시간) | 01, 02, 03, ... , 12                     |
| %p     | AM, PM               | AM, PM                                   |



#### 문자열을 날짜/시간 객체로

- datetime.datetime.strptime('문자열', 'format')

  ```python
  >>> date = datetime.datetime.strptime('20210304', '%Y%m%d')
  datetime.datetime(2021, 3, 4)
  ```

  

#### 날짜/시간 객체를 문자열로

- datetime객체.strftime('format')

  ```python
  >>> date.strftime('%Y-%m-%d')
  '2021-03-04'
  ```