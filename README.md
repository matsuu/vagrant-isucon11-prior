# vagrant-isucon11-prior

## Overview

ISUCON 11 Priorとほぼ同じ環境を構築するためのVagrantfileです。

## Usage

* vagrant実行環境を用意する
* このリポジトリ内のVagrantfileを手元に用意する
  * 必要に応じてVagrantfileを編集する
* Vagrantfileがあるディレクトリで`vagrant up`を実行する
  * サーバが1台起動
* itamaeによるプロビジョニングが完了したら`vagrant ssh`を実行する
  ```sh
   vagrant ssh
  ```
* ベンチマークを実行する
  ```sh
  sudo -i -u isucon
  bin/benchmarker
  ```

## 動作確認

macOS + VirtualBox 6.1.22 + Vagrant 2.2.16で動作確認済です。
VMWare Desktopでも動作するかもしれませんが未確認です。

## 本来の設定と異なるところ

* 本来のサーバはCPU 1コア、メモリ4GB構成だそうです。

## FAQ

### `vagrant up` でBoxのダウンロードが404 Not Foundのエラーになる

Vagrantのバージョンが古いためと思われます。
可能であれば最新バージョンのVagrantをご利用ください。
諸事情により古いVagrantしか使えない場合はVagrantfileの先頭に以下を設定してください。

```
Vagrant::DEFAULT_SERVER_URL.replace('https://vagrantcloud.com')
```

参考 https://github.com/matsuu/vagrant-isucon/pull/24

### virtualboxで以下のようなエラーメッセージが表示される

> The provider 'virtualbox' that was requested to back the machine
> 'default' is reporting that it isn't usable on this system. The
> reason is shown below:
>
> Vagrant has detected that you have a version of VirtualBox installed
> that is not supported. Please install one of the supported versions
> listed below to use Vagrant:
>
> 4.0, 4.1, 4.2, 4.3

Vagrantのバージョンが古い可能性があります。最新のVagrantを使用してください。

### vagrant upを実行するとvboxsfのエラーが表示される

> Failed to mount folders in Linux guest. This is usually because
> the "vboxsf" file system is not available. Please verify that
> the guest additions are properly installed in the guest and
> can work properly. The command attempted was:
>
> mount -t vboxsf -o uid=`id -u vagrant`,gid=`getent group vagrant | cut -d: -f3` vagrant /vagrant
> mount -t vboxsf -o uid=`id -u vagrant`,gid=`id -g vagrant` vagrant /vagrant
>
> The error output from the last command was:
>
> /sbin/mount.vboxsf: mounting failed with the error: No such device

[これと同じ現象](http://qiita.com/hapicky/items/a7f9d56588f96d005fad)と思われます。気にせず`vagrant provision`を実行してください。

### プログラムの動かし方がわからない

以下をご確認ください。

- [ISUCON 11 Prior Manual](https://github.com/isucon/isucon11-prior/blob/main/webapp/doc/MANUAL.md)

### ブラウザで動作確認ができない

Vagrantfileのネットワーク設定がデフォルトのままです。
`config.vm.network "forwarded_port", guest: 80, host: 8080` のコメントを外して `vagrant relaod` を実行後、ブラウザから `127.0.0.1:8080` にアクセスしてみてください。
