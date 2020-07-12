# Docker image for Tiny Tiny RSS (Raspberry pi 3B+)

## はじめに
これは https://github.com/JC5/docker-ttrss よりforkして、Raspberry pi 3B+ docker 上で動かすようにしたものです
イントラネット内で動かすことを想定しているため、セキュリティ等考慮していません。
すべて自己責任で

JC5さんありがとうございます！

## 環境
```bash
 $ uname -a
 Linux intra 4.9.35-v7+ #1014 SMP Fri Jun 30 14:47:43 BST 2017 armv7l GNU/Linux
 $ docker -v
 Docker version 18.06.3-ce, build d7080c1
```
## Quickstart

```bash
#コマンド１
docker run -d -e POSTGRES_USER=ttrss -e POSTGRES_PASSWORD=ttrss --name ttrssdb \
-v /mnt/exdisk/disk1/docker/datas/ttrss:/var/lib/postgresql/data postgres

#コマンド2
docker build -t ttrss .

#コマンド3
docker run -d -e DB_NAME=ttrss -e DB_USER=ttrss -e DB_PASS=ttrss -e DB_TYPE=pgsql \
-e SELF_URL_PATH=http://intra.haselab.com:8081/ --link ttrssdb:db -p 8081:80 ttrss

ブラウザーで http://intra.haselab.com:8081/へアクセス
 Username: admin  Password: password でログインできるはず。
 
 
```
## point
コマンド1
・postgresqlのユーザ、パスワードを設定します。あとでttrss本体から指定します
・-v オプションで、postgresqlのデータをホストPC側の領域を使うように設定しておきます。

コマンド2
Dockerfileを用いてビルドします

コマンド3
・コマンド1で指定したpostgresqlの情報を設定します
・SELF_URL_PATHでraspberrypiのサーバ名を指定します。
・raspberry pi のポート8081と　docker 上のポート80がつながっています。

Dockerfileについて
・基本、forkもとと同じですが、一箇所変更しています。
そのままでは、DB_TPYEを設定しているにも関わらず、設定が必要と出てきます。
そのためこのチェックを外すために、sanity_check.php を修正しています。



