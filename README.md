# docker-mirakc-recisdb-epgstation

* [recisdb-rs](https://github.com/kazuki0824/recisdb-rs)
* [mirakc](https://github.com/mirakc/mirakc) 
* [epgstation](https://github.com/l3tnun/EPGStation)

をdockerで動かす用のスクリプト.

## 依存関係

* linux 64bit
* docker/docker-compose
* 基本的なTS抜き用ハードウェア
  * PLEXなどのISDB-T/Sチューナー
  * B-CASカード/カードリーダー
* [px4_drv](https://github.com/tsukumijima/px4_drv)などのチューナー用ドライバ
* [pcscd](https://linux.die.net/man/8/pcscd)
* [recisdb-rs](https://github.com/kazuki0824/recisdb-rs)
* [tsukumijima/ISDBScanner](https://github.com/tsukumijima/ISDBScanner)(推奨)

## 環境構築手順

0. recisdb-rsがローカル環境で動作することを確認しておく.(方法はwebで調べると出てくる)
1. このレポジトリをクローン.
2. ``[compose.yml](./compose.yml) を自身の環境に合わせ修正.
3. [/conf/epgstation/config.yml.template](/conf/epgstation/config.yml.template)を
   `./conf/epgstation/config.yml` にコピーし編集
4. `/conf/mirakc/config.yml` を用意(ISDBScannerなどで生成)
5. 下記のコマンドを実行しサブモジュールを更新
   ```sh
   git submodule init
   git submodule update --init --recursive
   ```

以上.

## 起動

```sh
docker compose up -d
```

## 停止

```sh
docker compose down
```

## 更新

```sh
docker compose down
git pull main
git submodule update --init --recursive
docker compose build --pull
docker compose up -d
```
