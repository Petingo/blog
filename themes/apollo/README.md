README in [English](https://github.com/Petingo/hexo-theme-apollo/blob/master/README.md) / [中文](https://github.com/Petingo/hexo-theme-apollo/blob/master/README.zh-TW.md)
# Introduction
This is a theme for Hexo based on [hexo-theme-apollo](https://github.com/pinggod/hexo-theme-apollo) and [this fork](https://github.com/angelen10/hexo-theme-apollo).  Compare to the original theme, it comes with these features:
- gallery page produced by [unitegallery](https://github.com/vvvmax/unitegallery)
- tag
- empty page layout
- different font size from h2 to h5
- remove '#' in front of subtitle
- slight change on layout & color
## Demo
You can visit [my blog](petingo.me) to see how it works.
## Screenshots
![main-page](https://github.com/Petingo/hexo-theme-apollo/blob/master/screenshot/main-page.png?raw=true)
![gallery](https://github.com/Petingo/hexo-theme-apollo/blob/master/screenshot/gallery.png?raw=true)

## Install

#### Initialize blog & download theme
``` bash
hexo init Blog 
cd Blog 
npm install
npm install --save hexo-renderer-jade hexo-generator-feed hexo-generator-sitemap hexo-browsersync hexo-generator-archive
git clone https://github.com/Petingo/hexo-theme-apollo.git themes/apollo
```

#### Clean something useless
``` bash
rm -rf themes/landscape
rm -rf themes/apollo/screenshot
```

#### Change theme
in `config.yml`:
```yaml
theme: apollo

# show all articles in achieve page
# need to install hexo-generator-archive
archive_generator:
    per_page: 0
    yearly: false
    monthly: false
    daily: false
```
#### Latex
If you need LaTeX in your blog, you may have to change the markdown render engine to [hexo-renderer-markdown-it-plus](https://github.com/CHENXCHEN/hexo-renderer-markdown-it-plus) or the inline LaTeX `$..$` may not be rendered properly.
The KaTeX css style has been added to the theme, just run
```
npm un hexo-renderer-marked --save
npm i hexo-renderer-markdown-it-plus --save
```
And it should work.  For more infomation, check out their [github repo page](https://github.com/CHENXCHEN/hexo-renderer-markdown-it-plus).

## Create gallery page
``` bash
hexo new page gallery
```
Change `/source/gallery/index.md` to
```
---
layout: gallery
---
```
Go to flick and apply for an API-Key, create an photoset, and modify the config in `theme/_config.yml`.

If `random` is set as `true`, the order of the photos will be different everytime you visit the gallery page.
```yml
user_id: xxxxxxxxx@xxx
api_key: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
photoset_id: 'xxxxxxxxxxxxxxxxx'
random: false
```

## Create About Page
``` bash
hexo new page about
```
Change `/source/gallery/about.md` to
```
---
layout: about
---

And things you write here will be shown below the personal infomation block.
```
Change the link of the buttons in `/theme/layout/partial/about.jade/`

## Empty Page
You can create an empty page which contains nothing but the markdown file.  Just create a new page and set `layout` to `empty`.
An example is https://petingo.me/cp/.
```
---
title: 
layout: empty
---
```
## Other Features
### Meta Description

If you want to set meta description information, please set `desc` property and value to each post — the better method is setting default `desc` property to your scaffolds files, just like:

```md
title: Lorem ipsum dolor
date: 2015-12-31 14:49:13
desc: Lorem ipsum dolor sit amet, consectetur.
---

Lorem ipsum dolor sit amet, consectetur adipisicing elit. Totam, non numquam saepe ex ut. Deleniti culpa inventore consectetur nam saepe!
```

result:

```html
<meta name="description" content="Lorem ipsum dolor sit amet, consectetur.">
```

If there is no `desc` property or value, hexo-theme-apollo will use `page.title` and `page.author` instead of it. 

## post excerpt

If you want to show excerpt(core content of article) to your visitors, do add HTML comment tag `<!--more-->` before else content，and finally the tag will be parsed to be a variable which represents post excerpt by Hexo:

![https://cloud.githubusercontent.com/assets/9530963/14064341/0fa3c754-f432-11e5-8ad7-5d063d4a0886.png](https://cloud.githubusercontent.com/assets/9530963/14064341/0fa3c754-f432-11e5-8ad7-5d063d4a0886.png)

### Comment Plugin

Hexo-theme-apollo support two comment plugins: Disqus and Duoshuo. please set like this in your `themes/apollo/_config.yml`:

```yaml
disqus: seansun
```

### Danger Block

Use html tag with special class property to render block:

```html
<div class="tip">
    预处理器很强大，但它只是编写 CSS 的辅助工具。出于对扩展和维护等方面的考虑，在大型项目中有必要使用预处理器构建 CSS；但是对于小型项目，原生的 CSS 可能是一种更好的选择。不要肆意使用预处理器！
</div>
```

![danger](https://cloud.githubusercontent.com/assets/9530963/11359678/489a510c-92b9-11e5-9256-341cef6999b6.png)

## Last but not Least
Focus on blog posts, not blog's styles. Have fun :) !

## 