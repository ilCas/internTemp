# Ubuntu 17 環境構築メモ

## apt, apt-getでupdate時にエラー
17はサポートが切れている関係でリポジトリの設定を変更する必要がある。
```
$ cd /etc/apt
# バックアップ
$ sudo cp sources.list sources.list.bak
# リポジトリ設定の書き換え
$ sudo sed -i -e 's/jp.archive.ubuntu.com\|security.ubuntu.com/old-releases.ubuntu.com/g' /etc/apt/sources.list
```

## VMWare Tools
- 共通  
VMのディスプレイ設定を指定からホストの設定を使用に変更しておく。（VM起動中は変更不可）  
使うツールを変更する場合はインストール済みの方をアンインストールしておくこと。
- open-vm-tools  
VMWare ToolsよりはOpen VM Toolsを使うといいらしい？VMWareToolsよりは導入が楽。  
```
# インストール
$ sudo apt install open-vm-tools open-vm-tools-desktop
# アンインストールはapt remove
```
- VMWare Tools  
VMWare謹製
```
# マウントポイント作成
$ sudo mkdir /mnt/cdrom
# マウントと確認
$ sudo mount /dev/cdrom /mnt/cdrom
$ ls /mnt/cdrom
# tarの展開
$ cp /mnt/cdrom/VMwareTools-x.x.x-xxxx.tar.gz /tmp
$ tar xzvf /tmp/VMwareTools-x.x.x-xxxx.tar.gz -C /tmp/
$ cd /tmp/vmware-tools-distrib
# インストール
$ ./vmware-install.pl -d
# 再起動
$ sudo reboot
# アンインストール方法(コマンドがインストール済み)
$ vmware-uninstall-tools.pl
```

## Dockerインストール
Ubuntuへのインストール手順
- 必要なソフトのインストール
```
$ sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```
- 公開鍵登録、リポジトリ設定
```
# 鍵取得、設定
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
# 上記の確認
$ sudo apt-key fingerprint 0EBFCD88
# リポジトリ設定
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
- dockerのインストール
```
$ sudo apt-get update
$ sudo apt-get install -y docker-ce
$ docker version
```
- dockerコマンドの権限設定
```
$ sudo groupadd docker
$ sudo gpasswd -a $USER docker
$ sudo systemctl restart docker
$ reboot
```