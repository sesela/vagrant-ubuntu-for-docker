# vagrant-ubuntu-for-docker解説

## 事前準備
下記をインストールしておくこと

- VirtualBox
- Vagrant

プロキシを介してVagrantを使う場合はシステム環境変数に下記を追加すること
- HTTP_PROXY : http://<プロキシホスト名>:<プロキシポート>
- HTTPS_PROXY : http://<プロキシホスト名>:<プロキシポート>


## Vagrantのプラグインをインストール

### vagrant-vbguestをインストールする
`vagrant plugin install vagrant-vbguest` を実行する。
### プロキシを介している場合はvagrant-proxyconfをインストールする
`vagrant plugin uninstall vagrant-proxyconf` を実行する。

## Vagrantfileの修正

### プロキシ設定
- プロキシを介している場合、下記のようにプロキシ情報を設定してください。
- プロキシを介していない場合、プロキシのプラグインをインストールしていなければ修正の必要ないです。プラグインをインストールしている場合は「config.proxy.enabled  = false」にしてください。
```
  ...
  if Vagrant.has_plugin?("vagrant-proxyconf")
    config.proxy.enabled  = true  # => true; all applications enabled, false; all applications disabled
    config.proxy.http     = "http://<プロキシホスト名>:<プロキシポート>"
    config.proxy.https    = "http://<プロキシホスト名>:<プロキシポート>"
    config.proxy.no_proxy = "localhost,127.0.0.1"
  end
  ...
```

### その他設定
特に設定する必要はありませんが、
ポートフォワードを設定したい、IPアドレスを変更したい場合は想定する環境に合わせて設定してください。
また、Windowsで起動することを想定しているため`Encoding.default_external = 'SJIS'`と設定しています。エンコードでエラーが発生する場合はコメントアウトなり対応するエンコードに設定してください。

## 起動
Vagrantfileの配置されているディレクトリで`vagrant up`で起動します。

※初回起動時はprovisionが起動されdocker,docker-compose等がインストールされます。

## ゲストOSへアクセス
Vagrantfileで設定したIP（デフォルト: 192.168.33.10）へSSHでアクセスできます。「ID / PW : vagrant / vagrant」

## 停止
Vagrantfileの配置されているディレクトリで`vagrant halt`で停止します。

