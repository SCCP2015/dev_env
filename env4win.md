# How to install ubuntu-server14.04 on VMwarePlayer
## add new VM to library
VMwarePlayerは既にインストールされているものとする。以下のリンクから予めISOイメージファイルをダウンロードしておくように。
Download: [ubuntu-server14.04 ISOimage](http://releases.ubuntu.com/14.04/ubuntu-14.04.2-server-amd64.iso "iso")

1. VMwarePlayerの新規仮想マシン作成ウィザードを開き、「後でOSをインストール」を選択する。
2. Linux, Ubuntu64bitを選択する。
3. 仮想マシン名, 仮想マシンを格納するディレクトリは任意。
4. ディスク最大サイズは任意。仮想ディスクを単一ファイルとするか複数ファイルとするかは自由だが別にマシンを移動する予定なんて無いし単一ファイルとして格納して良さそう。
5. 「ハードウェアのカスタマイズ」を選択し、以下の項目を変更する。
* メモリ: 1GB -> 2GB(以上)
* プロセッサ: 1 -> 2～4
* CD/DVD: 物理ドライブ -> ISOイメージ
(先ほどダウンロードしたubuntu-server14.04のISOイメージのパスを指定する

以上で仮想マシンの作成は完了。

## installation

**※ デスクトップ環境のインストールが終わるまでは仮想マシン内でマウスポインタを使用することができないので注意。基本的にキーボード操作でインストールを行う。**

1. 作成した仮想マシンを再生する。OSの言語はEnglishを選択。
2. Install Ubuntu Serverを選択。
3. languageはEnglish、locationはJapanを選択。
(other/Asia/Japan と順に選択していく必要がある)
4. encodeはUS_UTF-8を選択
5. キーボードレイアウトはそれぞれ自分のキーボードのレイアウトを選択。
(日本語キーボードならJapanese, Japan)
6. hostnameは任意だがubuntuのままにしておくのがよさそう。
7. fullname, username, passwordは全て「vagrant」にする。その後脆弱なパスワードを使用してもいいのか聞かれるのでyesを選択。
8. ホームディレクトリの暗号化について聞かれるのでnoを選択。
9. timezoneがAsia/Tokyoになっていればyesを選択。
10. ディスクの分割方法は「全体を使用」(一番上)を選択。その次の項目もそのままReturnキーを押す。
11. ディスクへの変更の書き込みをするか聞かれるのでyesを選択。
12. HTTPプロキシ情報は空のままcontinueを選択。
13. セキュリティ更新は自動化を選択。(2つ目)
14. インストールするソフトウェアはOpenSSH, PostgreSQL, VM hostあたりを選んでcontinueを選択。
(正直必要なのかわからないけど一応インストールしておく)
15. GRUB boot loaderをインストールするのでyesを選択。

以上でインストールは完了。continueを選択しubuntuにログインする。

**※ 仮想マシンはマシンを内包するディレクトリをそのままコピーすることによって容易にバックアップすることができる。開発環境に変更が加えられる度にOSをインストールし直すのが面倒なのであれば、仮想マシンの保存パスにある該当ディレクトリを別の場所などにコピーしておくと良い。バックアップした仮想マシンをVMwarePlayerで再生したい場合には、ホーム画面の「仮想マシンを開く」から仮想マシンのイメージファイルを選択する。**
```
# Windows8.1でのデフォルトの仮想マシン保存パス(のはず)
C:\Users\username\Documents\Virtual Machines\
```

## set up an environment for SCCP

必要なパッケージの類をインストールするためにdev_env下のprovision.shを実行する。
* 途中で一度止まってCtrl-CかReturnを入力するように求められる場合があるが、その時はReturnを押す。
* また、途中でzshにログインして見かけプロセスが動いていないように見える時があるが、exitコマンドでzshからログアウトするとbashのほうでrubyのダウンロードとbuildが進んでいるはず。

```
# 実行するシェルをダウンロード、実行(最新版のdev_envのものであることを確認するように)
$ wget https://raw.githubusercontent.com/SCCP2015/dev_env/master/provision.sh
$ sudo sh ./provision.sh
```
全て終わったら、ubuntu-desktopをインストールして再起動する。
```
$ sudo apt-get install --no-install-recommends ubuntu-desktop
$ sudo reboot
```
再起動後はGUiが使えるようになるので、Ubuntu Software CenterのAccessoriesからGnome Terminalを選択しインストールする。

以上で「vagrant up」にあたる作業が全て終了となる。あとは作業ディレクトリに自分のリポジトリをcloneするなり改造するなり好きにしてOK。
