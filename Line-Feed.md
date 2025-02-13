## LF(Line-Feed) CRLF(Carriage Return Line-Feed)
window에서 `git add .`을 하는 순간   
`warning: in the working copy of 'venv/Scripts/activate', LF will be replaced by CRLF the next time Git touches it`식의 경고가 엄청나게 많이 발생   

해당 경고는 git이 파일에서 **줄 바꿈(Line Ending) 형식이 변경될 것** 이라고 알려주는 메시지

현재 해당 파일의 줄 바꿈이 **LF(Line Feed, \n)** 인데,
Git이 이를 **CRLF(Carriage Return + Line Feed, \r\n)** 로 변경할 예정이라는 의미
>

#### 해결방법
- 위의 경고를 무시해도 실행에는 문제 없음
- `git config --global core.autocrlf false` 을 입력해서 git의 자동 줄 바꿈 변환 끄기
- `#특정 프로젝트에만 적용 시 git config core.autocrlf true` `전역 적용 시 git config --global core.autocrlf true` 를 입력해서 줄 바꿈 자동 변환을 활성화

