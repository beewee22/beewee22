#Today I Read & Learned...#

#2020/07/29
- [더 나은 UX의 관리자 페이지를 위해](https://youngminz.netlify.app/posts/more-better-admin-page)
- [<번역>시니어 프론트엔드 개발자처럼 크롬 개발자도구 사용하기](https://junwoo45.github.io/2020-07-28-chrome_devtools/)
- [sed multi-line replace](https://mug896.github.io/sed-stream-editor/multiple_lines.html)

#2020/08/25
매일 하기로 하고 까먹었다;; 저번 주말(8/22)부터 [ReasonML](https://reasonml.github.io/ko/)을 들여다 보는 중. Reason React+NextJS를 섞어서 프로젝트 하나를 진행해 볼 생각.

#2020/08/29
- CRA(react-script)로 만든 프로젝트에서, 처음에 eslint를 따로 사용하겠다 선언하지 않은 경우 추후에 .eslintrc.* 파일을 생성해도 인식이 되지 않는다. 이럴 때 해결 방법은 .env 파일에 ```EXTEND_ESLINT=true``` 값을 추가하면 된다([블로그 게시물](https://medium.com/@michael.tandio/using-create-react-app-typescript-with-custom-eslint-rules-5fc5aec3ff10))

#2020/09/03
- CRA(react-script)로 만든 프로젝트에서, IDE에선 타입추론에 문제가 없었는데 실제 빌드타임에서 엄한 오류를 뿜고 있었다. 
문제: 문제가 되는 파일이 /src/@types/general.ts였고, typeRoots에 등록된 디렉토리에 있어서 바벨이 해당 파일의 Transfile을 건너뛰었기 때문에 eslint에서 해석하지 못해 생긴 문제였음.
교훈: 
  1. typeRoots에는 d.ts 파일만 넣자 -_-;;
  2. 혜성님처럼 에러메세지만 봐도 문제를 눈치채는 멋진 사람이 되자.
