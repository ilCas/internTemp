# Ubuntu 17 環境構築メモ

## apt, apt-getでupdate時にエラー
17はサポートが切れている関係でリポジトリの設定を変更する必要がある。
```
cd /etc/apt
# バックアップ
sudo cp sources.list sources.list.bak
# リポジトリ設定の書き換え
sudo sed -i -e 's/jp.archive.ubuntu.com\|security.ubuntu.com/old-releases.ubuntu.com/g' /etc/apt/sources.list
```

## VMWare Tools
- open-vm-tools
VMWare ToolsよりはOpen VM Toolsを使うといいらしい？VMWareToolsよりは導入が楽。  
なんか動作が不安定な気もする。
```
sudo apt install open-vm-tools open-vm-tools-desktop
```
- VMWare Tools
VMWare謹製
```
# マウントポイント作成
sudo mkdir /mnt/cdrom
# マウントと確認
sudo mount /dev/cdrom /mnt/cdrom
ls /mnt/cdrom
# tarの展開
tar xzvf /mnt/cdrom/VMwareTools-x.x.x-xxxx.tar.gz -C /tmp/
cd /tmp/vmware-tools-distrib
# インストール
./vmware-install.pl -d
# 再起動
sudo reboot
```