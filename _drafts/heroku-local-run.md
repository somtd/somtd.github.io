---
layout: post
title: herokuをlocalで実行する
date: 2016-05-11
categories: heroku 
---

## TL;DR
簡単なアプリを作ってherokuで動かしている。localで一旦挙動をテストしてからheroku上にpushする運用を試すメモ。

## heroku localの実行

以下を実行。

```
$  heroku local
```

Procfileに記述されているプロセスが全てlocalで実行されることになります。

## 環境変数の取り扱い

herokuの環境変数を扱っているアプリの場合、上記コマンドを実行すると下記のようなエラーが表示される。

```
[WARN] No ENV file found
```

local実行でも環境変数を設定する場合は別途環境変数用のファイルを用意する必要がある。

[ドキュメント](https://devcenter.heroku.com/articles/heroku-local#set-up-your-local-environment-variables)によると以下のファイルを作成して環境変数を記述しておくことで対応可能。

```
$ cat .env
```

記述例

```
S3_KEY=mykey
S3_SECRET=mysecret
```

上記ファイルはgitにプッシュしないように.gitignoreに追記しておくことを忘れないように。

環境変数は以下のコマンドで本番からコピーできる。

```
$ heroku config:get CONFIG-VAR-NAME -s  >> .env
```

再度、heroku localを実行して無事動くことを確認する。



