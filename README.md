# redmine-amazon-linux-ansible

Ansibleを使ってAmazon Linux にRedmineを構築するプレイブックです。

以下のCentOS用のプレイブックを参考に、Amazon Linux用へモディファイしました。

[redmine-centos-ansible](https://github.com/farend/redmine-centos-ansible)


## 想定環境

AWSのEC2に作成されたAmazon Linuxを対象とし、すでにインスタンスの作成が完了していることを想定しています。

## インストール手順

rootユーザーに昇格し、以下の手順を実行します。

### スワップ設定

t2.microでインスタンスを作成した場合は、メモリが1GBと多くありません。

インストール中に止まってしまうこともあるので、スワップを設定します。

お使いの環境にあわせて実施してください。

```
dd if=/dev/zero of=/swap bs=1M count=1024
chmod 600 /swap
mkswap /swap
swapon /swap
```

### pipとAnsibleのインストール

```
easy_install pip
pip install ansible
```

### EPELとGitのインストール

```
yum -y install epel-release
yum -y install git
```

### playbookのダウンロード

```
git clone https://github.com/mseninc/redmine-amazon-linux-ansible.git
```

### PostgreSQLに設定するパスワードの変更

ダウンロードしたplaybookから `redmine-servers` を開きます。

redmine-serversは以下のディレクトリにあります。

```
group_vars/redmine-servers
```

**db_passwd_redmine** を適当なパスワードに変更します。

### playbookの実行

ディレクトリを移動してplaybookを実行します。

```
cd redmine-amazon-linux-ansible
ansible-playbook -i hosts site.yml
```

インストールが完了したら、Webブラウザで　`http://< IPアドレス >/redmine` にアクセスします。

あとは、必要に応じて**iptables**でファイアウォールの設定を行ってください。

## オリジナルからの変更点

- SELinux 関連設定の削除 (Amazon Linux には初期状態で SELinux が導入されていないため)
- Firewalld 関連設定の削除（Firewalldがないため）
- iptables 設定を追記
- 日本語フォントの変更（該当するフォントが無いため）
- PostgreSQLのinitコマンドの変更
- pg_hba.confのパスを変更
- pythonのドライバを変更（バージョンの変更）
- redmine.confのアクセス制限変更（500エラー回避）

## ライセンス

MIT License

## 謝辞

CentOS用のプレイブックを作ってくださった、[ファーエンドテクノロジー株式会社](http://www.farend.co.jp/)さんに感謝いたします。