---
layout: post
title:  "Welcome to Jekyll"
date:   2019-12-17 20:10:15 +0900
categories: etc
---
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated. 당신은 이 포스트를 `_posts`라는 폴더 내에서 찾을 수 있다. 가서 그것을 편집하면 사이트를 스스로 변경할 수 있을 것이다. 당신은 다른 방식으로도 사이트를 변경할 수 있는데, 가장 흔한 방법은 `jekyll serve`를 실행해서 웹서버를 런칭한다음 새로고침해보는 것이다. 

Jekyll requires blog post files to be named according to the following format: 지킬은 블로그 포스트 파일이 다음과 같은 포맷을 가지고 이름 붙여져야 한다:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works. `YEAR`은 4자리 숫자이고 `MONTH`와 `DAY`는 두자리 숫자여야 하며 `MARKUP`은 파일 확장자여야 한다. 그 다음에 필요한 프론트매터를 포함할 수 있다. 

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/



## Jekyll 폴더 구성

- <a href="http://jihyeleee.com/blog/third-designer-can-make-jekyll-blog/"> 참고 </a>


- _config.hml
  - 환경설정 정보


- _drafts
  - 아직 게시하지 않은 post를 보관


- _includes
  - 재사용하기 위한 html 파일


- _layouts
  - 형태를 정의해놓은 html 파일들


- _data
  - 블로그에 사용할 수 있는 데이터를 모아놓을 수 있는 폴더


- _site
  - 건드려서는 안되는 자동 변환 파일들


