# Provisioning and Management of SHIRASAGI using Vagrant, Ansible

## What is this?

[shirasagi-ansible](https://github.com/hidakatsuya/shirasagi-ansible) は、[Ansible](http://www.ansible.com/home) + [Vagrant](https://www.vagrantup.com/) によって、 Ruby 製オープンソース CMS [SHIRASAGI](http://www.ss-proj.org/) の実行環境を構築・管理したり、ローカル環境で簡単に試すことを目的としています。具体的には、以下のことが可能です。

  1. SHIRASAGI が動作する仮想マシンをローカル環境で起動
  2. Ansible による本番/テスト環境の構成管理（本番環境の Playbook はこれから）

## 仮想マシンをローカル環境で起動

Vagrant を使って [Vagrant Cloud](https://vagrantcloud.com/) から [SHIRASAGI が予めインストールされた Box](https://vagrantcloud.com/hidakatsuya/boxes/shirasagi-centos65-x86_64) を取得してローカル環境で起動する方法です。

### 必要なソフトウェア

  * VirtualBox
  * Vagrant 1.5 以降

### 構成

インストールされるソフトウェアとそのバージョンは [shirasagi-centos65-x86_64 v0.4.0](https://vagrantcloud.com/hidakatsuya/boxes/shirasagi-centos65-x86_64/versions/1) を参照。
構築手順は [公式インストール手順](http://www.ss-proj.org/download/install.html) に従う。Ansible のソースコードは [ansible/roles](ansible/) を参照

### 手順

まず、 [VirtualBox](https://www.virtualbox.org/) と [Vagrant](https://www.vagrantup.com/) をインストール。
詳細な手順は、 [Google 等で検索](https://www.google.co.jp/search?q=vagrant+%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E6%89%8B%E9%A0%86&oq=vagrant+%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E6%89%8B%E9%A0%86&aqs=chrome..69i57.5209j0j7&sourceid=chrome&es_sm=119&ie=UTF-8#q=vagrant+%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)すると詳しい情報が見つけることができる

インストール後、ネットに繋がる環境で以下のコマンドを実行:

    $ vagrant init hidakatsuya/shirasagi-centos65-x86_64

完了後、同じディレクトリに作成された `Vagrantfile` をテキストエディタで開き、
`#config.vm.network "private_network", ip: "192.168.33.10"` 行のコメントを外して
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

ブラウザで 192.168.33.10:3000 にアクセスするとサイトが、192.168.33.10:3000/.mypage へアクセスすると管理ページのログイン画面が表示される。
以下のアカウントで管理者としてログインすることができる。

| ユーザID          | パスワード |
| -----------------| --------  |
| admin@example.jp | pass      |

## Ansible による本番/テスト環境の構成管理

### 必要なソフトウェア

  * VirtualBox
  * Vagrant
  * Ansible

### 動作確認済みの OS

  * MacOS X 10.9
  * Windows 8.1（予定）

### 準備

shirasagi-ansible のコードを取得:

    % git clone https://github.com/hidakatsuya/shirasagi-ansible.git

`git` が使えない場合は、[shirasagi-ansible](https://github.com/hidakatsuya/shirasagi-ansible) のページ右下にある "Download ZIP" ボタンから ZIP アーカイブをダウンロードして任意の場所に展開

#### Mac の場合

todo

#### Windows の場合

todo

### セットアップ

todo

### 使い方

todo

## TODO

  * README.md をちゃんと書く
  * テストを書く
  * 本番環境の Playbook を書く

## Copyright

&copy; 2014 Katsuya HIDAKA. See MIT-LICENSE for further details.
