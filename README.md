## How To Use 

- 로컬 실행

```bash
bundle exec jekyll s
```

## 게시물 작성 규칙

- **게시물은 _post 폴더 하위**에 작성
  - 폴더 규칙은 1차 카테고리 / 2차 카테고리 / 게시물명으로 작성
  - 게시물은 **YYYY-MM-DD-게시물명.md**로 작성
  - 기본적인 Markdown 문법 외 **Jekyll의 Front Matter를 사용 금지**
- **카테고리**는 1차 카테고리와 2차 카테고리로 구성됨
  - ```yaml
    categories: [TOP_CATEGORIE, SUB_CATEGORIE]
    ```
- **author**는 **author.yml**에 등록된 id로 작성
  - ```yaml
    roky:
      name: rookedsysc
      url: https://github.com/rookedsysc
    ```
  - 게시물에 작성시 author: roky로 작성

## 공식 사이트 

https://chirpy.cotes.page/