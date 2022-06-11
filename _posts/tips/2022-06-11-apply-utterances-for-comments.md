---
title: jekyll chirpy 테마에 utterances comments(댓글) 창 추가
author: June
date: 2022-06-11 19:21:00 +0900
categories: [Tips]
tags: [tip, jekyll, chirpy, comments, utterances]
toc: true
toc: true
math: true
mermaid: true
comments: true
---
# utterances란? [[공식 사이트](https://utteranc.es)]
## 1. utterances에 대하여
utterances는 GitHub 문제를 기반으로 하는 가벼운 댓글 위젯입니다. 블로그 댓글, 위키 페이지 등에 GitHub 문제를 사용할 수 있습니다.
## 2. 특징
- 오픈 소스. 🙌
- 추적 없음, 광고 없음, 완전 무료. 📡🚫
- GitHub issue에 모든 모든 데이터가 저장됩니다. 🔓
- GitHub를 지원하는 CSS 툴킷인 Primer로 스타일이 지정되었습니다. 💅
- 다크 테마를 지원합니다. 🌘

# utterances 적용하기
## 1. 댓글이 저장될 깃 허브 저장소를 생성한다.
   - 일반적으로 git pages(blog)를 사용할 경우 그 Repository를 사용하면 된다.

## 2. utterance Install
   - https://github.com/apps/utterances 에서 Install을 클릭한다.
     - 해당 브라우저로 github에 로그인되어 있는 상태에서만 Install 버튼이 노출된다!
![utterance-install](/posts/tips/screencapture-github-apps-utterances.png)
   - 댓글이 모든 Repository의 Issue에 등록 되게 할지, 아니면 특정 Repository의 Issue에만 등록 되게 할지 선택한다. 대부분의 경우 아래를 선택하면 된다.
   - 아래를 선택하고, 1에서 생성한(혹은, 기존에 가지고 있던) Repository를 선택한다.  
![utterance-permissions](/posts/tips/screencapture-github-apps-utterances-installations-new-permissions.png)

## 3. 설정 작성
   - Install이 완료된 후 나오는 페이지를 작성한다.
   - Repository는 1에서 생성한(혹은, 기존에 가지고 있던) Repository를 입력한다.
   - Blog Post ↔️ Issue Mapping는 매핑 방법(key)을 선택한다.
     - 여러 선택지가 있지만 왠만하면 pathname을 선택하면 된다.
   - 기타
     - label 입력
     - Theme를 선택
   - Enable Utterances에 생성되어 나온 내용을 복사한다.
     - chirpy 등 일부 테마에서는 필요 없다.
![utteranc-es](/posts/tips/screencapture-utteranc-es.png)

## 4. 적용
### chirpy 테마의 경우
- _config.yml 파일 수정
```yml
comments:
  active: utterances        # 사용할 댓글 서비스: utterances라고 적음.
  utterances:
    repo:         ijung/ijung.github.io # 1에서 생성한(혹은, 기존에 가지고 있던) Repository.
    issue_term:   pathname # 3의 Blog Post ↔️ Issue Mapping에서 선택한 방법.
```

### 그 외 jekyll 테마
- 대부분의 경우는 위의 chirpy 테마의 경우와 같이 _config.yml 파일에 comments 관련 설정 부분이 있으며, 거기서 설정 가능하다.
- 그 외의 경우, "_layouts/post.html" 등 댓글 구현을 담당하는 html 파일에 3에서 복사한 내용을 적어 준다.

## 5. 끝
![complete](/posts/tips/screencapture-complete.png)


