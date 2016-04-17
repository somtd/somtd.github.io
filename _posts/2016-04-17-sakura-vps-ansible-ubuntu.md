---
layout: post
title: さくらVPS上にAnsibleでUbuntuの環境構築するところまで
date: 2016-04-17
categories: sakura VPS Ansible Ubuntu
---

## TL;DR

さくらのVPS上にDocker環境を整えるためにまずはAnsibleで自動化できるところまで自動化する試み。
実際はそんなに何回も叩くことは無いと思うが、自分の理解のためにメモを残す。

## Ubuntu用のAnsible playbook

CentOSはよく見かけるがUbuntu用のplaybookの書き方でちょっと詰まってしまった。
いい例を探していたところ以下のリポジトリを発見。
typoが多く若干不安ではあるが、Ansibleのベストプラクティスに沿ったディレクトリ構成となっているなど参考になりそうなので、こちらを元に構築することにした。

[zenzire/ansible-bootstrap-ubuntu](https://github.com/zenzire/ansible-bootstrap-ubuntu)

`hosts`を以下のように変更。

```hosts
[server]
160.xx.xxx.xxx #自分のVPSのIPアドレス

```

`server.yml`は以下のように編集。

```
---
hostname: 'ubuntu-sakura'
collectd_server_name: 'collectd-web.mydomain.com'
locale: 'en_US.UTF-8'
language: 'en_US:en'
timezone: 'Asia/Tokyo'
ntp_server: 'pool.ntp.org'

```

## adminユーザーを追加する


Ubuntuのサーバー初期化時にユーザー名をadminとせずに作成してしまったので、このタイミングでadminユーザー追加を行う。

READMEに従うとユーザーの追加を行うためには以下のコマンドを実行する。

```
$ ansible-playbook user.yml --ask-sudo
```

その前に鍵認証でSSH接続できるようにしておく。

*サーバー側（Ubuntu側）*

```
$ mkdir .ssh
$ chmod 700 .ssh
```

ローカル側（Ansibleのコードがある環境）からサーバー側にid_rsa.pubを送る。

*ローカル側*

```
$ scp ./id_rsa.pub user@160.xx.xxx.xxx:.ssh/
```

*サーバー側*

サーバー側でauthorized_keyに追加する。

```
$ cat id_dsa.pub >> authorized_keys
$ chmod 600 authorized_keys
```

## 気を取り直してplaybookを実行

*ユーザー追加*

```
$ ansible-playbook user.yml --ask-sudo
SUDO password:
Enter username: admin
Enter password: 
confirm Enter password: 
Enter id_rsa.pub path [~/.ssh/id_rsa.pub]: 
Add user to sudoers group (y/n) [n]: y
```

*Bootstrap実行*

```
$ ansible-playbook bootstrap.yml --ask-sudo
SUDO password:
```

*Reboot実行*

```
$ ansible-playbook reboot.yml --ask-sudo
SUDO password:
Are you sure you want to reboot server (yes/no)? [no]: yes
```

最初の鍵でのSSH接続を登録しておけば問題なく進められそう。

中身を見て勉強しつつ、次はDockerインストールを試す。




