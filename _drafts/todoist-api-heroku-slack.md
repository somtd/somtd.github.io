---
layout: post
title: TodoistのAPIを使って日々のタスクの消化具合をherokuを使ってslackに通知する
date: 2016-05-16
categories: todoist python heroku slack
---

## TL;DR

slackにtodoistから日々のやることリストやったことリストを流す処理をheroku上から定期的に実行できるようにするメモ。

↓こんな感じで。 

![image](https://gyazo-s3.herokuapp.com/1463493192.png) 

todoistのofficialでAPIラッパーが公開されているのでそれを使う。 

[Doist/todoist-python](https://github.com/Doist/todoist-python)

## ソースコード

動作環境

```
version: Python3.5.1 
heroku上で動作 
```

[somtd/todoistapp](https://github.com/somtd/todoistapp)


## 主な機能

主な機能の説明。 
スクリプトを実行すると、 
- 昨日完了したタスク 
- 今日完了したタスク 
- 今日が期限のタスク 
- 今日が期限以外で期限の日付が設定されているタスク 
を指定したslackチャンネル上に投稿する。 


`slackapp.py`を見ていただくと大体何をしているかがわかるかと。 

## heroku上での実行手順

```
$ mkdir todoistapp 
$ cd todoistapp 
```

heroku上にアプリを作成。 

```
$ heroku create todoistapp 
```

## herokuの環境変数の設定

```
$ heroku config:set SLACK_CHANNEL=<YOUR SLACK CHANNEL ID> 
$ heroku config:set SLACK_TOKEN=<YOUR SLACK TOKEN> 
$ heroku config:set TODOIST_TOKEN=<YOUR TODOIST TOKEN> 
```

### slackチャンネルのIDの探し方

こちらに詳しく書いてあった。
自分のtokenを使ってチャンネル一覧を取得してくれば分かる。

[Slack API/channels.list](https://api.slack.com/methods/channels.list)

## デプロイ

```
$ git push heroku master　
```

## 毎日１回だけ処理を行わせたい場合

こちらのadd-onを追加する。 
[Heroku Scheduler](https://devcenter.heroku.com/articles/scheduler) 

![scheduled](https://gyazo-s3.herokuapp.com/1463493425.png) 

毎朝9時ごろにこんな感じで届く。 

![todoist_image](https://gyazo-s3.herokuapp.com/1463493091.png) 



