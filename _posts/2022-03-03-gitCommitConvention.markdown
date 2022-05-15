---
layout: post
title:  "git commit convention"
date:   2022-03-03 19:31:29 +0900
categories: Git
---

### 들어가며
평소에 git commit Message에 깊은 고민을 하지 않고 적용해왔는데<br> 
협업을 하면서 내 commit Message를 보면서 무슨 생각을 할까? 라는 생각이 들었습니다.<br>
제가 생각해봐도 가독성이 떨어지며 유지보수하는데에 어려움이 있다 판단했고 어떻게하면<br>
협업을 더 잘할 수 있을까 고민하여 git commit convention 을 설정하게 되었습니다.<br>
[Udacity Git Commit Message Style Guide 참조](https://udacity.github.io/git-styleguide/)

### 메세지 구조

```
type : 제목
 
body : 본문

footer : 꼬리말
```

### Type
- feat : 새로운 기능 추가
- fix : 버그 수정
- docs : 문서 수정
- style : 코드 포맷 변경, 세미콜론 누락, 코드 변경 없는 겨우
- refactor : 코드 리펙토링
- test : 테스트 코드, 리펙토링 테스트 코드 추가
- chore : 빌드 업무 수정, 패키지 매니저 수정

### Subject
- 제목은 50자 이내, 대문자로 작성, 마지막에 마침표를 붙이지 않는다.
- 과거형이 아닌 명령어로 작성 (Fixed -> Fix)

### Body
- 선택사항이며, 모든 커밋에 본문 내용을 작성할 필요는 없다.
- 부연설명이 필요할 경우 작성해준다.
- 72자를 넘기지 않고 제목과 구분되기 위해 한칸을 띄워 작성한다.

### Footer
- 선택사항이며, 모든 커밋에 꼬리말을 작성할 필요는 없다.
- issue tracker id를 작성할때 사용한다.

### Udacity Git Commit Message Example
```
feat : Summarize changes in around 5@ characters or less

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. In some contexts, the first line is treated as the
subject of the commit and the rest of the text as the body. The
blank line separating the summary from the body is critical (unless
you omit the body entirely); various tools like `log`, `shortlog`
and `rebase` can get confused if you run the two together.

Explain the problem that this commit is solving. Focus on why you
are making this change as opposed to how (the code explains that).
Are there side effects or other unintuitive consequences of this
change? Here's the place to explain them.

Further paragraphs come after blank lines.

 - Bullet points are okay, too

 - Typically a hyphen or asterisk is used for the bullet, preceded
   by a single space, with blank lines in between, but conventions
   vary here

If you use an issue tracker, put references to them at the bottom,
like this:

Resolves: #123
See also: #456, #789
```

### My Example

```
feat : 사용자 문의 게시판 기능 추가

2022.02.21 기획팀의 추가 기획에 의한 사용자 문의 게시판 기능 추가
 - commons-io, commons-fileupload 라이브러리 추가
```

### 마치며
commit Message를 잘쓰면 더 나은 가독성, 리뷰 프로세스, 유지보수 등이 용이하다는걸 느꼈습니다.<br>
앞으로도 협업을 중요시 하고 성장하는 개발자가 되겠습니다.
