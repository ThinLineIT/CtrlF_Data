# Package Manager
- Package Manager 정의
- Package Manager 대표적인 종류(npm, yarn)
- npm/yarn의 차이점

# You can answer
- Package Manager이 무엇인지 알 수 있다.
- npm과 yarn의 차이점을 알 수 있다.

## Package Manager 정의
  - 패키지(코드의 배포를 위해 사용되는 코드의 묶음)의 설치, 업데이트, 수정, 삭제를 안전하고 편리하게 관리하는 툴이다.
  
## Package Manager 종류
### npm
- Node Package Manager의 약자로 이하, 노드 패키지 매니저라고 불린다.
- https://www.npmjs.com 에서 필요한 라이브러리를 내려받아 설치, 삭제 등의 관리를 해주는 프로그램이다.

- npm은 node_modules 폴더에 라이브러리를 내려받아 저장하고 package.json 파일에 설치된 라이브러리를 정보를 적어서 따로 보관한다.
- 즉, 실제 라이브러리와 라이브러리 명세 파일을 따로 보관한다.   
- Why?
    ```   
      1. node_modules에 저장되는 라이브러리의 용량이 매우 큼(1GB이상인 경우도 있음)   
      2. 개발자 간의 프로젝트를 공유할 때 단순히 라이브러리 명세와 핵심코드만을 전송하여 유연하게 코드를 공유할 수 있게끔 하기 위함
    ```
### yarn

- facebook에서 만든 것으로 npm과 같이 js 패키지 매니저이다.
- npm의 일관성, 보안, 빌드 시 성능 등에 문제를 해결하기 위해 만들어졌다.

## npm/yarn의 차이점
### 1. 속도(Speed)
- npm : 순차적으로 패키지를 다운받으므로 속도가 느리다.(v5.0부터는 yarn의 속도와 별 차이 없음)
- yarn : 병렬적으로 패키지를 설치하므로 속도가 빠르다.

### 2. 보안성(Security)
- npm : 패키지가 설치될 때 자동으로 코드와 의존성을 실행하도록 허용했기에 사용하기에 편리하지만 보장된 정책 없이 패키지가 존재 할 수 있으므로 위험도가 높다.
- yarn : yarn.lock이나 package.json으로 부터 설치만 할 수 있고, yarn.lock은 모든 장치들이 같은 패키지를 설치를 보증하기 때문에 버전 차이에 대한 문제를 현저히 낮춘다.

# Reference
[npm vs yarn](https://www.keycdn.com/blog/npm-vs-yarn)   
[npm/yarn 명령어](https://velog.io/@kysung95/%EA%B0%9C%EB%B0%9C%EC%83%81%EC%8B%9D-npm%EA%B3%BC-yarn)