---
layout: post
title: 自分用Gyazoをheroku+S3で運用する
date: 2016-04-10
categories: heroku AWS S3
---

## TL;DR
スクショの共有やBlog用に毎度毎度画像をアップロードするのが面倒臭いと思っていたので自分用のGyazoを動かしてS3に画像をホストする仕組みをherokuを介して運用する。

こちらの便利な仕組みを使わせてもらいます。
[https://github.com/soramugi/gyazo-s3](https://github.com/soramugi/gyazo-s3)

## ローカル環境の準備

```
$ git clone https://github.com/soramugi/gyazo-s3.git
$ cd gyazo-s3
```

herokuのstackがCeder-14になっている関係でRubyのバージョンを`2.1.4`にする。

[参考：Ruby Versions and Cedar 14](https://devcenter.heroku.com/articles/ruby-versions-and-cedar-14)


```
$ rbenv install 2.1.4
$ rbenv local 2.1.4
$ rbenv exec gem install bundler
$ NOKOGIRI_USE_SYSTEM_LIBRARIES=1 bundle install --path vendor/bundle
```

## herokuにdeployする

```
$ heroku create gyazo-s3
$ heroku config:set SECRET_KEY_BASE=$(bundle exec rake secret)
$ heroku config:set S3_BUCKET=S3_BUCKET
$ heroku config:set AWS_ACCESS_KEY_ID=AWS_ACCESS_KEY_ID
$ heroku config:set AWS_SECRET_ACCESS_KEY=AWS_SECRET_ACCESS_KEY
$ heroku config:set GYAZO_ID=$(cat ~/Library/Gyazo/id)
$ heroku config:set TITLE=GyazoS3
$ git push heroku master
$ heroku run bundle exec rake db:migrate
```

## Gyazoアプリ側の設定

`/Applications/Gyazo.app/Contents/Resources/script`を下記のように修正する。

```
#HOST = 'upload.gyazo.com'
HOST = 'gyazo-s3.herokuapp.com'
bundle_identity = get_info('CFBundleIdentifier')
if bundle_identity == 'com.gyazo.mac.teams'
   #CGI = '/teams/upload'
   CGI = '/items'
```

## サムネイルが表示されない

```
heroku open
```
してみたもののなぜかサムネイルが表示されない。
[paperclip](https://github.com/thoughtbot/paperclip)のバージョンが古いのかなーとか思っているのでついでに直す。

![Thumbnails](https://gyazo-s3.herokuapp.com/1460287523.png)

↑は無事S3に上がったスクリーンショット。