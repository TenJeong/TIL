# Git 관리대상에서 제외하기

## gitignore
기본적으로 clone이 된 프로젝트에도 적용하기 편하게 만들기 위해서 gitignore를 많이 사용한다.

```
# 로그 파일 제외
logs/
*.log

# 빌드 결과물 제외
/build/
*.o
*.class

# 환경 설정 파일 제외
.env
config/local.json
```

## ./git/info/exclude
git 관리대상에서 제외해야하면서, 보안 등으로 인해 해당 파일의 존재 자체를 알리고 싶지 않을 때는 ./git/info/exclude 파일을 사용한다.

역할과 사용방법은 .gitignore와 동일하지만, 로컬 저장소 전용으로서 원격 저장소에 push되지 않는다.