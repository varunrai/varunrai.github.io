---
layout: post
title: "Jekyll Code Highlight CSS Issue"
date: 2021-07-25 00:00:00 +0800
categories: jekyll css issue
tags: jekyll "code highlight" css
---

Recently, I started looking at the Jekyll Github pages to host the blog posts and got it working without much issues with the guide posted by [Barry Clark](https://github.com/barryclark/jekyll-now).

After i started working on it. Found that there was an issue with the syntax highlighting css that i renders double frame around the block code syntax (inline code works fine).
![jekyll code syntax issue](/images/b-jekyll-css-issue.jpg "Jekyll code syntax css issue")

The resolution was rather simple. The simple fix for the highlight.css resolved the issue:
![jekyll code syntax issue fix](/images/b-jekyll-css-fix.jpg "Jekyll code syntax css fix")

[Fix on Github](https://github.com/varunrai/varunrai.github.io/commit/15a2ef02ef93fdb9e7930fa2e3aeda8a6edee8ca#diff-c48705f6e89d98170a0ecca22a72d7ebaae4837e51253b9ccbfdd67ce8fd658d)
