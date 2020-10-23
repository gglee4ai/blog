# blog

* hugo 및 blogdown를 이용한 개인 블로그
* repository는 public으로 설정해야 함
* Rmarkdown 수정을 위해서는 blogdown:::serve_site()를 실행한 후 작성
* github page 이용을 위해서 config.toml에서 publishDir = "docs"로 변경
* 확실한 publish를 위해서는 docs 폴더를 제거한 후 hugo로 compile하고, push할 것
