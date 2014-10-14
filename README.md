# SHIRASAGI by Vagrant, Ansible

[shirasagi-ansible](https://github.com/hidakatsuya/shirasagi-ansible) は、[Ansible](http://www.ansible.com/home) + [Vagrant](https://www.vagrantup.com/) によって、 Ruby 製オープンソース CMS [SHIRASAGI](http://www.ss-proj.org/) の実行環境を構築・管理したり、ローカル環境で簡単に試すことを目的としています。具体的には、以下のことが可能です。

  1. SHIRASAGI が動作する仮想マシンをローカル環境で起動
  2. Ansible による本番/テスト環境の構成管理

(2) でも (1) と同様に、ローカルに動作環境をビルドすることが可能です。

## 仮想マシンをローカル環境で起動

Vagrant を使って [Vagrant Cloud](https://vagrantcloud.com/) から [SHIRASAGI が予めインストールされた Box](https://vagrantcloud.com/hidakatsuya/boxes/shirasagi-centos65-x86_64) を取得してローカル環境で起動する方法です。

### 必要なソフトウェア

  * VirtualBox
  * Vagrant 1.5 以降

### 構成

[shirasagi-centos65-x86_64 v0.4.0](https://vagrantcloud.com/hidakatsuya/boxes/shirasagi-centos65-x86_64/versions/1) を参照

### 手順

まず、 [VirtualBox](https://www.virtualbox.org/) と [Vagrant](https://www.vagrantup.com/) をインストール（詳細な方法については、サイトやブログを参照）

インストール後、ネットに繋がる環境で以下のコマンドを実行:

    $ vagrant init hidakatsuya/shirasagi-centos65-x86_64

完了後、同じディレクトリに作成された `Vagrantfile` をテキストエディタで開き、
`# config.vm.network "private_network", ip: "192.168.33.10"` 行のコメントを外して
任意の IP アドレスを設定（デフォルト `192.168.33.10` のままでも良い）

``` ruby
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # :
  # config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.network "private_network", ip: "192.168.33.10"
  # :
end
```

続いて、以下のコマンドを実行して仮想マシンを起動 (10 分程度かかる):

    $ vagrant up

最後に、仮想マシンへログインし SHIRASAGI を起動:

    $ vagrant ssh
    [vagrant]$ cd /var/www/shirasagi
    [vagrant]$ bundle exec rake unicorn:start
    [vagrant]$ exit

ブラウザで 192.168.33.10:3000 にアクセスするとサイトが、192.168.33.10:3000/.mypage へアクセスすると管理ページのログイン画面が表示される


## Ansible による本番/テスト環境の構成管理

### 必要なソフトウェア

  * VirtualBox
  * Vagrant
  * Ansible

### 動作確認済みの OS

  * MacOS X 10.9
  * Windows 8.1（予定）

### 手順

shirasagi-ansible のコードを取得:

    % git clone https://github.com/hidakatsuya/shirasagi-ansible.git

`git` が使えない場合は、[shirasagi-ansible](https://github.com/hidakatsuya/shirasagi-ansible) のページ右下にある "Download ZIP" ボタンから ZIP アーカイブをダウンロードして任意の場所に保存

#### Mac の場合

(TODO)

#### Windows の場合

(TODO)

### 使い方

(TODO)

## 今後やること

  * README.md をちゃんと書く
  * テストを書く
  * 本番環境の Playbook を書く

## Copyright

&copy; 2014 Katsuya HIDAKA. See MIT-LICENSE for further details.
