---
layout: post
title: '[Solved] Jekyll 프레임워크 사용 ~.min.js not found. 에러가 발생'
date: 2024-10-12 22:30 +0900
categories: [Issue, Github Blog]
tags: [github, github blog, jekyll, chirpy, jekyll-theme-chirpy, .min.js not found]     # TAG names should always be lowercase
---

## ERROR `/assets/js/dist/home.min.js' not found. 
Github Blog 작성 (chirpy테마) 및 로컬 서버의 기동 과정에서 발생한 에러에 대해 알아보자.  

---
### 로컬환경
- Ruby+Devkit 3.1.3-1
- jekyll 4.3.4
- Bundler version 2.5.21

---
### jekyll테마
- jekyll-theme-chirpy

---
### 이슈 해결
1. 리소스 경로 확인  
   - 리소스 파일 존재 여부 확인:  
     잘못된 파일명이 사용되었는지 점검.  
     프로젝트 디렉토리에 리소스 파일이 실제로 존재하는지 확인.  

2. 로컬환경 버전 확인  
   - ruby >= 2.5.0  
  
3. 프로젝트 내부에서 해당 Path 파일을 사용하고 있는 곳 확인  
   - `rollup.config.js`의 `build`function에서 확인.  

4. 검색을 통해 해결방안 모색 (2가지 방안 확인)  
`rollup.config.js`의 움직임만 해결하면 좋은 것 같아 _첫번째_ 방법으로 진행.  

   - 공통점  
     - **node.js 모듈 설치** 가 필수조건임을 확인.  
       - node v20.18.0  
       - npm 10.8.2  
     - `.gitignore` 파일에 `assets/js/dist` 부분을 주석처리 **(블로그 배포할 때를 위한 처리)**

   - 첫번째  
     - `assets/js/dist` 폴더를 수동 작성  
     - `npm install rollup` && `npx rollup --version` 다운로드  
     - `rollup.config.js` -> `rollup.config.cjs`로 변경 후, `npx rollup -c --bundleConfigAsCjs`  
     - `assets/js/dist` 폴더에 `min.js` 파일이 성공적으로 출력됨을 확인  

   - 두번째  
     - `npm install` && `npm run build`  

> 두번째 방법으로 진행할 시, `'NODE_ENV'은(는) 내부 또는 외부 명령, 실행할 수 있는 프로그램, 또는 배치 파일이 아닙니다.` 라는 메세지가 출력 될 경우 `npm install -g -win-node-env` 로 에러 해결 후 다시 빌드을 실행
{: .prompt-info }
