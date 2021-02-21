## UNIX Standard Stream



UNIX의 프로그램이 시작되면 자동으로 아래 세 개의 스트림이 열림 

1. STDIN(standard input, 표준 입력)

2. STDOUT(standard output, 표준 출력)

3. STDERR(standard error, 표준 오류)

이 세 가지를 Standard Stream(표준 스트림)이라고 한다.



#### stdin

- 프로그램에 들어가는 데이터를 프로그램에 나타내주는 스트림

- read명령을 사용



#### stdout

- 프로그램의 출력 데이터를 기록하는 스트림 
- write명령을 사용



#### stderr

- stdout과 다른 별도의 출력 스트림이며 프로그램의 오류메세지나 진단을 출력하기 위한 스트림



#### 추가

- 모든 명령이 입력과 출력을 필요로 하지는 않음

- 여기서 데이터는 일반적으로 문자열이지만, 파일이나 다른 형식이 될 수도 있음