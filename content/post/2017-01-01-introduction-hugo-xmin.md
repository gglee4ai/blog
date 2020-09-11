---
title: Introduction of hugo-xmin theme
author: Yihui Xie
date: '2017-01-01'
slug: introduction-hugo-xmin
categories: [Web]
tags: [hugo]
draft: no
---

## HUGO XMIN

### _Keep it simple, but not simpler_

[**XMin**](https://github.com/yihui/hugo-xmin) is a Hugo theme written by [Yihui Xie](https://yihui.name) in about four hours: half an hour was spent on the Hugo templates, and 3.5 hours were spent on styling. The main motivation for writing this theme was to provide a really minimal example to beginners of Hugo templates. This XMin theme contains about 130 lines of code in total, including the code in HTML templates and CSS (also counting empty lines).

```bash
find . -not -path '*/exampleSite/*' \( -name '*.html' -o -name '*.css' \) | xargs wc -l
```

```bash
       5 ./layouts/404.html
      12 ./layouts/_default/single.html
      20 ./layouts/_default/list.html
    13 ./layouts/_default/terms.html
       0 ./layouts/partials/foot_custom.html
       0 ./layouts/partials/head_custom.html
       9 ./layouts/partials/footer.html
      20 ./layouts/partials/header.html
      51 ./static/css/style.css
       7 ./static/css/fonts.css
     137 total
```

I can certainly further reduce the code, for example, by eliminating the CSS, but I believe a tiny bit of CSS can greatly improve readability. You cannot really find many CSS frameworks that only contain 50 lines of code.

Although it is a minimal theme, it is actually fully functional. It supports pages (including the home page), blog posts, a navigation menu, categories, tags, and RSS. With [a little bit customization](https://github.com/yihui/hugo-xmin/blob/master/exampleSite/layouts/partials/foot_custom.html), it can easily support LaTeX math expressions, e.g.,

`$${\sqrt {n}}\left(\left({\frac {1}{n}}\sum _{i=1}^{n}X_{i}\right)-\mu \right)\ {\xrightarrow {d}}\ N\left(0,\sigma ^{2}\right)$$`

All pages not under the root directory of the website are listed below. You can also visit the list page of a single section, e.g., [posts](/post/), or [notes](/note/). See the [About](/about/) page for the usage of this theme.

## About Hugo XMin

**XMin** is the first Hugo theme I have designed. The original reason that I wrote it was I needed a minimal example of Hugo themes when I was writing the  [**blogdown**](https://github.com/rstudio/blogdown) book. Basically I wanted a simple theme that supports a navigation menu, a home page, other single pages, lists of pages, blog posts, categories, tags, and RSS. That is all. Nothing fancy. In terms of CSS and JavaScript, I really want to keep them minimal. In fact, this theme does not contain any JavaScript code at all, although on this example website I did introduce some JavaScript code (still relatively simple anyway). The theme does not contain any images, either, and is pretty much a plain-text theme.

The theme name "XMin" can be interpreted as "**X**ie's **Min**imal theme" (Xie is my last name) or "e**X**tremely **Min**imal theme".

## config.toml

For this example site, I defined permalinks for two sections, `post` and `note`, so that the links to pages under these directories will contain the date info, e.g., `https://xmin.yihui.name/post/2016/02/14/a-plain-markdown-post/`. This is optional, and it is up to your personal taste of URLs.

```toml
[permalinks]
    post = "/post/:year/:month/:day/:slug/"
    note = "/note/:year/:month/:day/:slug/"
```

You can define the menu through `menu.main`, e.g.,

```toml
[[menu.main]]
    name = "Home"
    url = "/"
    weight = 1
[[menu.main]]
    name = "About"
    url = "/about/"
    weight = 2
[[menu.main]]
    name = "Categories"
    url = "/categories/"
    weight = 3
[[menu.main]]
    name = "Tags"
    url = "/tags/"
    weight = 4
[[menu.main]]
    name = "Subscribe"
    url = "/index.xml"
```

Alternatively, you can add `menu: main` to the YAML metadata of any of your pages, so that these pages will appear in the menu.

The page footer can be defined in `.Params.footer`, and the text is treated as Markdown, e.g.,

```toml
[params]
    footer = "&copy; [Yihui Xie](https://yihui.name) 2017"
```

## Custom layouts

There are two layout files under `layouts/partials/` that you may want to override: `head_custom.html` and `foot_custom.html`. This is how you inject arbitrary HTML code to the head and foot areas. For example, this site has a file `layouts/partials/foot_custom.html` to support LaTeX math via MathJax and center images automatically:

```html
<script src="//yihui.name/js/math-code.js"></script>
<script async src="//cdn.bootcss.com/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

<script async src="//yihui.name/js/center-img.js"></script>
```

You can certainly enable highlight.js for syntax highlighting by yourself through `head_custom.html` and `foot_custom.html` if you want.

If you do not like the default fonts (e.g., `Palatino`), you may provide your own `static/css/fonts.css` under the root directory of your website to override the `fonts.css` in the theme.

## Other features

I could have added more features to this theme, but I decided not to, since I have no intention to make this theme feature-rich. However, I will teach you how. I have prepared several examples via pull requests at [https://github.com/yihui/hugo-xmin/pulls](https://github.com/yihui/hugo-xmin/pulls), so that you can see the implementations of these features when you check out the diffs in the pull requests. For example, you can:

- [Enable Google Analytics](https://github.com/yihui/hugo-xmin/pull/3)

- [Enable Disqus comments](https://github.com/yihui/hugo-xmin/pull/4)

- [Enable highlight.js for syntax highlighting of code blocks](https://github.com/yihui/hugo-xmin/pull/5)

- [Display categories and tags on a page](https://github.com/yihui/hugo-xmin/pull/2)

- [Add a table of contents](https://github.com/yihui/hugo-xmin/pull/7)

- [Add a link in the footer of each page to "Edit this page" on Github](https://github.com/yihui/hugo-xmin/pull/6)

To fully understand these examples, you have to read [the section on Hugo templates](https://bookdown.org/yihui/blogdown/templates.html) in the **blogdown** book.

## Design philosophy

Lastly, a few words about my design philosophy for this theme: I have been relying on existing frameworks like Bootstrap for years since I'm not really a designer, and I was always scared by the complexity of CSS.

When I started writing this theme, I asked myself, "_What if I just write from scratch?_" No Bootstrap. No Normalize.css. I don't care about IE (life could be so much easier without IE) or inconsistencies among browsers (for personal websites). As long as the theme looks okay in Chrome, Firefox, and Safari, I'm done. Thanks to the simplicity of Markdown, you cannot really produce very complicated HTML, and I think styling the HTML output from Markdown is much simpler than general HTML documents. For example, I do not need to care much about form elements like textareas or buttons.

After I finished this theme, I started to wonder why I'd need `normalize.css` at all (it sounds like a religious belief). The default appearance of modern browsers actually looks pretty good in my eyes, after I tweak the typeface a little bit.

Compared to inconsistencies across browsers, I care much more about these properties of HTML elements:

- Tables should always be centered, and striped tables are easier to read especially when they are wide. Tables should not have vertical borders.
- An image should be centered if it is the only child element of a paragraph.
- The `max-width` of images, videos, and iframes should be `100%`.

I hope you can enjoy this theme. The source code is [on Github](https://github.com/yihui/hugo-xmin). Happy hacking!
