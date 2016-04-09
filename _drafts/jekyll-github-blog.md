---
layout: post
title: Jekyll+GithubでBlogを運用する
date: 2016-04-09
categories: Github Jekyll
---


## TL;DR

実装や環境構築のメモをどこかに残しておきたいのと、定期的に訪れる「ブログ作り直したい欲」に負けてJekyllつかって構築。
放置していたGithub Pagesと独自ドメインもついでに設定しなおして使用開始。

[https://pages.github.com/](https://pages.github.com/)
[https://jekyllrb.com/](https://jekyllrb.com/)
[Github Pagesの独自ドメイン](https://help.github.com/articles/using-a-custom-domain-with-github-pages/)

## Jekyllのインストール

```
$ gem install jekyll
$ jekyll new blog
$ cd blog
~/blog $ jekyll serve
```

`http://localhost:4000`にアクセスするとローカルで動く様子が見える。


## Github Pagesにプッシュする

放置していた自分のGithub Pagesのリポジトリと連携させる。

```
cd blog 
$ git init
$ git remote add git@github.com:somtd/somtd.github.io.git
$ git add .
$ git commit -m "Initial commit"
$ git push origin master
```

これで[http://somtd.github.io/](http://somtd.github.io/)から見られるようになる。

## 独自ドメインの設定

[こちらのヘルプ記事](https://help.github.com/articles/setting-up-an-apex-domain/#configuring-a-records-with-your-dns-provider)を参考に設定。お名前.comでAレコードの設定を行う。

CNAMEファイルに以下を追記する。

```
blog.matsuda.so
```

これで[http://blog.matsuda.so/](http://blog.matsuda.so/)から見られるようになる。