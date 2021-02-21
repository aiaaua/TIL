## Git Error : CRLF will be replaced by LF

#### Unix와 Window의 한 줄의 끝

- Unix : LF(Line Feed)
- Window : CR(Carriage Return), LF(Line Feed)

따라서 unix와 window를 왔다갔다 하면 둘 중에 어느쪽을 선택할지 git이 혼란스러워함



#### 해결방법 

이를 자동 변환해주는 기능 켜기

```git
git config --global core.autocrlf true
```

(만약 해당 프로젝트에만 적용하고 싶다면 `--global` 빼기)



#### [참조](https://blog.jaeyoon.io/2018/01/git-crlf.html)







