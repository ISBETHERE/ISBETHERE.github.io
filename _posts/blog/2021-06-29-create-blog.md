---
title: Jekyll, minimal-mistakes 테마를 적용하기
categories:
  - Blogs
tags:
  - Jekyll
  - minimal-mistakes
toc: true
---

## 설정
- Github Pages 호스팅을 위해 `${GITHUB_ID}.github.io` 라는 이름으로 리파지토리를 생성해야 한다
- Gem을 이용해서 Jekyll 을 설치한다


## 테마 적용
minimal-mistake [빠른 시작 가이드](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/)를 참고하여 진행한다.  
[minimal-mistake](https://github.com/mmistakes/minimal-mistakes) Repository에서 소스를 가져와 옮긴다.  
옮긴 후 다음 과정을 진행한다.  

### 불필요 내용 제거
minimal-mistakes-jekyll repository에서 소스를 가져왔다면 불필요한 내용을 제거한다.  
- .editorconfig
- .gitattributes
- .github
- /docs
- /test
- CHANGELOG.md
- minimal-mistakes-jekyll.gemspec
- README.md
- screenshot-layouts.png
- screenshot.png

### _config 파일 설정
`_config.yaml` 파일에서 아래와 값들을 설정해준다


#### 기본 구성
```
# Site Settings
minimal_mistakes_skin    : "dark"
title                    : "상행전용 웹개발자"
name                     : "상행전용 웹개발자"
description              : "회사에서는 월급 받고 집에서는 취미로 코딩하는 개발자의 블로그"
url                      : "https://.github.io"
baseurl                  : # 서브 경로가 있는 경우 기재
```

#### teaser와 logo 그림 파일
```
teaser                   : "/assets/images/bio.jpg"
logo                     : # 최상단 메뉴 바에 사이트 로고 넣기
```

#### 댓글
블로그 댓글 기능 disqus, discourse, facebook, staticman, utterances 정도가 대표적이다.   
GitHub Pages 자체적으로 댓글을 제공하고 있지 않기때문에 보통 외부 댓글 서비스를 연결해주는 방식을 사용한다.  
```
comments:
  provider               : # 블로그 댓글 기능 false (default), "disqus", "discourse", "facebook", "staticman", "staticman_v2", "utterances", "custom"
  disqus:
    shortname            : # 블로그 댓글 기능 https://help.disqus.com/customer/portal/articles/466208-what-s-a-shortname-
  discourse:
    server               : # https://meta.discourse.org/t/embedding-discourse-comments-via-javascript/31963 , e.g.: meta.discourse.org
  facebook:
    # https://developers.facebook.com/docs/plugins/comments
    appid                :
    num_posts            : # 5 (default)
    colorscheme          : # "light" (default), "dark"
  utterances:
    theme                : # "github-light" (default), "github-dark"
    issue_term           : # "pathname" (default)
```

#### 저자 설정
minimal-mistakes theme은 사이트 좌측 사이드바에 기본으로 사이트 저자 소개를 설정한다.
```
# Site Author
author:
  name             : "isbethere"
  avatar           : "/assets/images/bio-photo.jpg"
  bio              : "상행전용 🧑‍💻"
  location         : "South Korea"
  email            :
  links:
    - label: "Email"
      icon: "fas fa-fw fa-envelope-square"
      url: "oksky2957@daum.net"
    - label: "Blog"
      icon: "fas fa-fw fa-link"
      url: "https://.github.io"
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: "https://.github.io"
```

#### 저자 설정 - Footer
```
# Site Footer
footer:
  links:
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/isbethere"
```

#### 블로그 표시방법 설정
```
# Outputting
permalink: /:categories/:title/
paginate: 5 # 첫 페이지에보여줄 최근 게시물 수를 지정
paginate_path: /page:num/
timezone: Asia/Seoul # https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
```


## 포스트 쓰기
포스트는 특정 제목으로 작성되어야 한다.   
`YYYY-MM-DD-{TITLE}.md` 형식으로 파일명을 작성한다 . 
메타데이터를 위한 yaml 부분을 상단에작성하고, 본문을 위한 마크다운 부분을 그 아래에 작성한다.
```
---
title: Jekyll, minimal-mistakes 테마를 적용하기
categories:
  - Blogs
tags:
  - Blog
  - Jekyll
  - minimal-mistakes
toc: true
---
```