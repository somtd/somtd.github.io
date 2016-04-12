---
layout: post
title: herokuで動いているアプリのstateをslackで確認する
date: 2016-04-11
categories: heroku slack papertrail
---

## TL;DR
herokuは一定時間アクセスがないとDynoがサスペンドして、次にアクセスするまで待ち時間が発生し、レスが悪くなってしまうそうです。
動かしているアプリはそこまでクリティカルではないので常時起動している必要もないのですが、とりあえずは状態監視しておける状況を作るためにherokuにaddonを追加しました。

このaddonを使います。

[https://elements.heroku.com/addons/papertrail](https://elements.heroku.com/addons/papertrail)

## addonを追加する

以下を実行。

```
$  heroku addons:create papertrail
```

クレジットカードを登録しておかないと以下のエラー。addonが利用できるプランに変更しておく。


```
!    Please verify your account to install this add-on plan (please enter a credit card) For more information, see https://devcenter.heroku.com/categories/billing Verify now at https://heroku.com/verify
```

登録して改めて実行すると、すぐにアプリ用のpapertrailが作られる。

```
$ heroku addons:open papertrail
```

実行するとブラウザからpapertrailのダッシュボードが開く。

## LogからSearchする対象を登録する

Logを眺めていると、stateが変わるタイミングで以下のLogが出力されているようなので、そこを検索対象として登録する。

```
Apr 10 22:37:52 gyazo-s3 heroku/web.1:  State changed from down to starting 
Apr 10 22:38:02 gyazo-s3 heroku/web.1:  State changed from starting to up 
Apr 10 23:10:59 gyazo-s3 heroku/web.1:  State changed from up to down 
```

とりあえず`State changed from`を登録した。

## Slackで監視の通知を受け取る

追加した検索条件から新たにAlertを登録する。
Alertの通知手段が選択できるので、Slackを選択する。

![alert settings](https://gyazo-s3.herokuapp.com/1460423780.png)

Slack DetailsのところにWebhookのURLを登録するところがあるので、URL記入欄の下のリンクから自分のslack上でWebhookを作成する。

WebhookのURLを貼り付けて登録完了。

![alert sample](https://gyazo-s3.herokuapp.com/1460424110.png)

無事に通知がきた。