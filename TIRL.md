# Today I Read & Learned...

# 2020/07/29
- [더 나은 UX의 관리자 페이지를 위해](https://youngminz.netlify.app/posts/more-better-admin-page)
- [<번역>시니어 프론트엔드 개발자처럼 크롬 개발자도구 사용하기](https://junwoo45.github.io/2020-07-28-chrome_devtools/)
- [sed multi-line replace](https://mug896.github.io/sed-stream-editor/multiple_lines.html)

# 2020/08/25
매일 하기로 하고 까먹었다;; 저번 주말(8/22)부터 [ReasonML](https://reasonml.github.io/ko/)을 들여다 보는 중. Reason React+NextJS를 섞어서 프로젝트 하나를 진행해 볼 생각.

# 2020/08/29
- CRA(react-script)로 만든 프로젝트에서, 처음에 eslint를 따로 사용하겠다 선언하지 않은 경우 추후에 .eslintrc.* 파일을 생성해도 인식이 되지 않는다. 이럴 때 해결 방법은 .env 파일에 ```EXTEND_ESLINT=true``` 값을 추가하면 된다([블로그 게시물](https://medium.com/@michael.tandio/using-create-react-app-typescript-with-custom-eslint-rules-5fc5aec3ff10))

# 2020/09/03
- CRA(react-script)로 만든 프로젝트에서, IDE에선 타입추론에 문제가 없었는데 실제 빌드타임에서 엄한 오류를 뿜고 있었다. 

문제: 문제가 되는 파일이 /src/@types/general.ts였고, typeRoots에 등록된 디렉토리에 있어서 바벨이 해당 파일의 Transfile을 건너뛰었기 때문에 eslint에서 해석하지 못해 생긴 문제였음.
교훈: 
  1. typeRoots에는 d.ts 파일만 넣자 -_-;;
  2. 혜성님처럼 에러메세지만 봐도 문제를 눈치채는 멋진 사람이 되자.

# 2020/09/06
- snowpack, 11ty를 처음으로 사용해봤음. 
  의도는 마크다운 파일을 작성하면 자동으로 사이트에 올라갔으면~
  프로젝트 인원 모두가 웹 프론트엔드 저작이 가능한 환경이 아니었기에, 저작툴로 가장 간단한 깃헙 위지윅 에디터를 사용할 것을 상정했음.
- 구글 드라이브에 작성한 문서를 깃헙 페이지로 퍼블리시 해주는 파이프라인도 있다고 한다. 정확한 이름은 모르지만, 참고 해볼 것.

- git에서 .gitignore 업데이트 없이 로컬에서 특정 파일만 변경을 무시하고 싶을 때:

  ```git update-index --skip-worktree <file-list>```를 입력하면 됨.
    
  해제하고 싶으면 ```git update-index --no-skip-worktree <file-list>```를 입력.

  [스택오버플로우 답변 참고](https://stackoverflow.com/a/1753078)

# 2021/10/27
- 엄청 오랜만에 업뎃함.. 그동안 배운게 없는게 아니고 쓰는걸 잊었는데 막상 또 안쓰니까 배운걸 까먹는 악순환 ㅋ
- **문제의 발단**: 현재 프로젝트를 vite-react-typescript 템플릿으로 만들어서 잘 쓰고 있었는데, 테스트를 위해 Jest를 설치함
- **증상**: Jest에서 vite에서 환경변수를 참조할 때 사용하는 ```import.meta.env...``` 구문을 해석하지 못해서 에러가 남
- **원인**: vite는 esm을 사용하고, 환경변수 주입도 일반적인 nodejs의 ```process.env``` 안이 아닌 ```import.meta.env``` 내에 주입되는데, 이건 nodejs에서 아직 기본적으로 지원하는 구문이 아님
- **그래서 뭘 했나?**:
  1. `Babel이라면 해결해 줄거야!` 라는 헛된 희망을 품고 `babel-jest` 와 `babel-plugin-transform-import-meta` 를 설치해서 `jest.config.ts` 에 설정함 => 안됨
  2. package.json의 node `run script`에 jest 실행시 node에 `--experimental-vm-modules` 플래그를 줘서 실행해봄 
    => 오류 메세지가 바뀌었는데, `import.meta`를 모듈의 밖에서 사용할 수 없다고 나옴
    => 이 부분을 찾아보니, `package.json`에 ```"type": "module"```로 바꾸고 js를 mjs로 바꾸라는 소리가 나옴. 우리가 갈 길과는 다른 곳이구나 깨닫고 다른 방법을 찾음
  3. `import.meta`를 접근 가능한 상황에만 참조하면 되지 않을까? 하고 ```process.env.NODE_ENV === 'test'```일 경우엔 ```process.env```를 리턴하도록 변경해봤으나, 렉시컬 환경엑서 이미 오류를 터뜨림(즉, 실제로 실행되지 않는 코드여도 ```import.meta``` 구문이 있으면 에러 남 ㅡㅡ
- **어케 해결함?**:
  인류가 문자를 발명하고 수 없이 많은걸 문서로 남겨놨는데 왜 영장류로 태어난 나는 문서를 안 읽지? Jest 문서에 이미 답이 있었음 ㅡㅡ (https://jestjs.io/docs/configuration#extensionstotreatasesm-arraystring)
  `jest.config.ts`에 ```extensionsToTreatAsEsm: ['.ts', '.tsx']``` 설정을 추가한 뒤(js 추가하면 에러남), 아까 node `run script`에 ```"test": "node --experimental-vm-modules node_modules/jest/bin/jest.js"``` 요런 식으로 ```--experimental-vm-modules``` 옵션 추가해서 실행해주면 해결된다.

