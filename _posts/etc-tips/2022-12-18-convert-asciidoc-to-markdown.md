---
title: asciidoc(adoc)을 markdown(md)로 변환하기 (convert adoc to md)
author: June
date: 2022-12-18 18:08:00 +0900
categories: [기타 팁]
tags: [asciidoc, adoc, markdown, md, ]
toc: true
math: true
mermaid: true
comments: true
---
1. pandoc, asciidoc 설치
    - linux

    ```console
    sudo apt install pandoc asciidoc
    ```

    - mac os (with homebrew)

    ```console
    brew install asciidoc pandoc
    ```

1. asciidoc(adoc)을 docbook(xml)으로 변환

    ```console
    asciidoc -b docbook {asciidoc(입력) 파일 명}.adoc
    ```

    - 결과로 {asciidoc(입력) 파일 명}.xml 파일이 생성된다.

1. docbook(xml)을 markdown(md)으로 변환

    ```console
    pandoc -f docbook -t markdown_strict {xml(입력) 파일 명}.xml -o {출력 파일 명}.md
    ```

    - 결과로 {출력 파일 명}.md 파일이 생성된다.
    - 완료
