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

### Requirements

  * VirtualBox
  * Vagrant
  * Ansible

### Supported Platforms

  * MacOS X 10.9
  * Windows 8.1

## Getting Started

Vagrant + Ansible でローカルに仮想マシンを起動する手順

### Step1. VirtualBox と Vagrant のインストール

それぞれ、公式サイトから環境にあったファイルをダウンロードしてインストール

### Step2. Ansible のインストール

#### MacOS

  * [By Homebrew](http://docs.ansible.com/intro_installation.html#latest-releases-via-homebrew-mac-osx)
  * By easy_install, pip
```
$ sudo easy_install pip
$ sudo pip install paramiko PyYAML jinja2 httplib2
$ sudo pip install ansible
```

#### Windows

この手順は以下のページを参考にしている:
http://www.resilvered.com/2014/04/running-ansible-on-windows.html

##### [Cygwin](https://www.cygwin.com/) のインストール

[Cygwin Installation](https://cygwin.com/install.html) より、32bit の場合は `setup-x86.exe` を 62bit の場合は `setup-x86_64.exe` をダウンロードして実行

インストールウィザードの中で、以下のパッケージを選択すること:

  * wget
  * python
  * python-crypto
  * python-openssl
  * python-setuptools
  * git
  * vim
  * openssh
  * openssl
  * openssl-devel
  * libsasl2
  * ca-certificates

ダウンロードミラーは `http://ftp.yz.yamagata-u.ac.jp` を選択すると良い。後は、ウィザードに従う

##### Ansible のインストール

Cygwin Terminal 又は Cygwin64 Terminal を起動し、 PyYAML と Jinja2 をインストール

PyYAML:

    $ cd ~
    $ wget https://pypi.python.org/packages/source/P/PyYAML/PyYAML-3.11.tar.gz
    $ tar -xvf PyYAML-3.11.tar.gz
    $ cd PyYAML-3.11
    $ python setup.py install

Jinja2:

    $ cd ~
    $ wget https://pypi.python.org/packages/source/J/Jinja2/Jinja2-2.7.3.tar.gz
    $ tar -xvf Jinja2-2.7.3.tar.gz
    $ cd Jinja2-2.7.3
    $ python setup.py install

Ansible を [GitHub](https://github.com/ansible/ansible) からダウンロードし `/opt/ansible` へインストール:

    $ git clone https://github.com/ansible/ansible /opt/ansible
    $ cd /opt/ansible
    $ git submodule update --init --recursive

`~/.bash_profile` に以下の内容を追記して `$ source ~/.bash_profile` で反映

```
ANSIBLE=/opt/ansible
export PATH=$PATH:$ANSIBLE/bin
export PYTHONPATH=$ANSIBLE/lib
export ANSIBLE_LIBRARY=$ANSIBLE/library
```

### Step3. shirasagi-ansible のコードを取得

任意の場所にて（ホームディレクトリで実行したとする）:

    $ git clone https://github.com/hidakatsuya/shirasagi-ansible.git

### Step4. shirasagi-ansible のセットアップ

#### SSH の設定

shirasagi-ansible をインストールしたディレクトリにある ssh.conf.example を ssh.conf としてコピー:

    $ cd /path/to/shirasagi-ansible
    $ cp ssh.conf.example ssh.conf

Vagrant で起動するだけなら、 ssh.conf の `IdentityFile` を Vagrant が生成する秘密鍵 `insecure_private_key` のパスに変更すれば良い。`insecure_private_key` は `$HOME/.vagrant.d/insecure_private_key` に作成されている。

Windows （Cygwin）の場合は以下のように設定する:

```
Host vagrant
   :
  IdentityFile /cygdrive/c/Users/username/.vagrant.d/insecure_private_key
   :
```

### Step5. Vagrant で仮想マシンを起動する

    $ cd /path/to/shirasagi-ansible
    $ vagrant up

上記 `vagrant up` で、以下の処理が実行される:

  1. CentOS 6.5 の box ファイルがダウンロードされ、 `box add` される
  2. CentOS 6.5 の box がプライベートネットワーク上で `192.168.33.10` として起動
  3. Ansible の Playbook が全て実行され、SHIRASAGI の動作環境の構築とサンプルデータがロードされる

完了後、ブラウザで 192.168.33.10:3000 アクセスするとサイトが、 192.168.33.10:3000/.mypage へアクセスするとログイン画面が表示される。ユーザアカウントは、以下の通り:

| ユーザID          | パスワード |
| -----------------| --------  |
| admin@example.jp | pass      |

## Usage

### Ansible Playbook を再度実行する

    $ cd /path/to/shirasagi-ansible
    $ ansible-playbook ansible/site.yml

### Production 環境で Playbook を実行する

TODO

## TODO

  * テストを書く
  * 本番環境の Playbook を書く

## Copyright

&copy; 2014 Katsuya HIDAKA. See MIT-LICENSE for further details.
