<div id="top"></div>

<!-- Shields/Logos -->
[![License][license-shield]][license-url]
[![Issues][issues-shield]][issues-url]
[![Discord][discord-shield]][discord-url]
<!-- Project Logo -->
<p align="center">
  <a href="https://github.com/sebanc/brunch" title="Brunch">
   <img src="../Images/terminal_icon-512.png" width="128px" alt="Logo"/>
  </a>
</p>
<h1 align="center">Windowsでブランチをインストールする</h1>

<!-- Installation Guides -->
# USBインストール
このガイドはWindowsを使ってUSB（または他のディスク）にBrunchをインストールするためのものです。 シングルブートする場合もこのガイドが必要です。 まずWindowsを起動し、下のドロップダウンをクリックしてください。

<details>
  <summary>クリックしてブランチUSBガイドを開く</summary>

### 必要条件
- 管理者アクセス。
- ターゲットディスク/USBは16GB以上であること。
  - また、Windowsのインストールに約16GBの空き容量が必要です。
- WSL2によるLinuxのインストール
- `pv`, `tar`, `unzip` および `cgpt` パッケージである.
- ブランチを起動するための[互換PC][互換性]。
- Linuxターミナルを初級レベルで理解していること。
  - このガイドは、このプロセスをできるだけ簡単にすることを目的としているが、基本を知っておくことは期待されている。

### リカバリー
1. お使いのCPUに適したリカバリーをダウンロードしてください。 以下のリストを参考にしてください。 最新のBrunchリリース番号と一致するリカバリーを選択する必要はありません。
  
#### Intel
* 第8世代と第9世代： Intelの場合は"[shyvana][recovery-shyvana]"、Celeronの場合は"[bobba][recovery-bobba]"
* 第10世代: "[jinlon][recovery-jinlon]".
* 第11世代以上: "[voxel][recovery-voxel]".
#### AMD
* Ryzen: "[gumboz][recovery-gumboz]".

リカバリーは上記のリンクをクリックしてご覧ください。 また、[cros.tech][cros-tech]にアクセスし、ご希望のリカバリーを検索することでも見つけることができます。

ご希望のリカバリーを選択した後、特定のリリースを選択することができます。 投稿されたリリースは現在のリリースより遅れている場合がありますが、これは通常のことで、後で現在のリリースにアップデートすることができます。 通常、利用可能な最新リリースを使用することをお勧めします。

### ファイルの収集
2. ブランチのファイルはGitHubリポジトリからダウンロードしてください。他のサイトにあるファイルやオンラインビデオにリンクされているファイルは使用しないでください。 [releases tab][releases-tab]はGitHubのメインページの右側の列の一番下にありますが、一般的には [latest release][latest-release]を使うことが推奨されています。

リリースをダウンロードする際は、リリース記事の下にあるアセットからbrunch...tar.gzファイルを選択してください。 ソースコードファイルは必要ありませんので、ダウンロードしないでください。

続行する前に、WSL2を使ってマイクロソフトストアからLinuxディストロをインストールし、ディストロをセットアップして使用できる状態にしておく必要があります。 システムによってはセットアップが複雑な場合があるので、オンラインリソースを参照してください。

### ターミナルの準備
3. Brunchリリースと選択したChromeOSリカバリーの両方のファイルをダウンロードしたら、WSL2を起動します。
4. pv、cgpt、tar、unzipがインストールされていることを確認する。

```sudo apt update && sudo apt -y install pv cgpt tar unzip```
  * 私の例では、DebianとUbuntuベースのディストロ用のパッケージマネージャーである`apt`を使用している。 Arch を使っている場合は、cgpt にアクセスするために [vboot-utils][vboot-utils] が必要で、それ以外をインストールするには別のパッケージマネージャーが必要になるかもしれません。

4b. Linux のリリースによっては、上記の依存関係をインストールするために `universe` リポジトリが必要になる場合があります。 もし、依存関係が利用できないというエラーが表示された場合は、このコマンドで `universe` レポジトリを追加し、その後でもう一度前のステップを試してください。

```sudo add-apt-repository universe```
  
5. すべての依存関係がインストールされたら、ファイルをダウンロードしたディレクトリに `cd` してください。
  * username`をあなたの*Windows*ユーザー名に置き換えてください。
  * linuxのターミナルは大文字と小文字を区別します。

```cd /mnt/c/Users/username/Downloads```
  
6. tar`を使ってBrunchアーカイブを取り出す。
  * `brunch_filename.tar.gz` を実際のファイル名に置き換えてください。

```tar zxvf brunch_filename.tar.gz```
  
7. `unzip`を使用してChromeOSリカバリを解凍する。
  * chromeos_filename.bin.zip`を実際のファイル名に置き換える。

```unzip chromeos_filename.bin.zip```

完了すると、ブランチアーカイブから4つの新しいファイルと、次のステップで使用するリカバリビンができます。

### ブランチ

8. ファイルの準備ができたら、Brunchのインストールは完了です。
  * 前と同じように、`chromeos_filename.bin`をbinファイルの実際のファイル名に置き換える。

```sudo bash chromeos-install.sh -src chromeos_filename.bin -dst chromeos.img```

スクリプトが確認を求めてくる。 インストールの準備ができたら、プロンプトに `yes` と入力してください。

ディスクの速度によってはインストールに時間がかかるかもしれません。 GPTヘッダーのエラーがいくつか出るかもしれませんが、無視してください。

インストールが終了すると、ChromeOSがインストールされたことが報告されます。 ターミナルを閉じる前に、追加のエラーがないことを確認してください。 エラーがなければ、問題ありません！

### USBを作る

9. WSL2は直接ディスクにアクセスできないので、WSL2でimgを作成し、[Rufus][rufus-link]や[Etcher][etcher-link]などの別のプログラムを使ってディスクをUSBに書き込みます。お好みのプログラムを開き、ダウンロードフォルダ内のchromeos.imgを選択し、USBに書き込みます。

### 次のステップ
  
USBまたは2台目の内蔵ディスクにインストールした場合は、Brunchを起動する準備ができているはずです。 USBにインストールした場合は、接続したまま再起動してください。 最初の起動に時間がかかるのは普通です。

* 最初の起動は、[changing kernels][change-kernels]や[framework options][framework-options]のような重要なものをセットアップするのに最適なタイミングである。
* もし何か問題があれば、[Brunch Configuration Menu][edit-brunch-config] でパッチや解決策を確認することを強くお勧めします。
* この時点で、お使いのデバイスは、実際のサイズに関係なく、インストールが14GBしかないと誤って表示する可能性があります。これは、**Ctrl + Alt + F2**を使用して起動画面で開発者シェルを開くことで修正できます。
  * `root`としてログインしてください。パスワードはないはずです。
  * `resize-data`と入力し、終了したらPCを再起動する。 これで報告されたサイズが正確になるはずです。

## セキュアブート
  
10. セキュアブートが有効になっている場合、起動時に「検証に失敗しました： (15) アクセス拒否」というブルースクリーンが表示されることがある。
  * USBから直接キーを登録するには、「OK」→「Enroll key from disk」→「EFI-SYSTEM」→「brunch.der」→「Continue」を選択し、再起動します。

  </details>
  
***
 
# シングルブートインストール
このガイドはBrunch USBを使ってBrunchをディスクにインストールするためのものです。このガイドではインストールを開始するためにBrunch USBが必要です。まず、Brunch USBを起動し、下のドロップダウンをクリックしてください。

<details>
  <summary>クリックしてシングルブートガイドを開く</summary>

### 必要条件
- 管理者アクセス
- ターゲットディスクは最低16GB必要です。
- 動作するBrunch USB
- Brunchを起動するための[互換PC][互換性]。
- 入門レベルのLinuxターミナルの理解
  - 本ガイドはこのプロセスをできるだけ簡単にすることを目的としていますが、基本的な知識は必要です。

### ターゲットディスクの選択
  
1. ChromeOSにログインし、**Ctrl + Alt + F2**でTTY2ターミナルに切り替え、`chronos`としてログインする。

2. 続行する前に、インストール先のディスクを確認してください。 続行する前に**絶対に確認してください**。このインストールは、他のパーティションを含め、ディスク上の**すべてを消去します**。 ディスクは 16 GB 以上でなければインストールに失敗します。 どのディスクがターゲットかを決定する方法はいくつかありますが、この例では `lsblk` を使用します。
  
```lsblk -e7```
  
このコマンドはディスクとその上のパーティションを表示します。 サイズと現在マウントされているかどうかも表示されます。 この情報を使って、どのディスクがターゲットかを決定する。
  
***
  
#### ヒント:
  
* ターゲットは zram やループデバイスではありません。
* PCによっては、ディスクを正しく表示する前にRAIDを無効にする必要があります。
* このインストールでは、USBはHDDやSSDと同じように扱われます。
* ディスクに EFI マウントポイントがある場合、そのディスクが起動ディスクとなります。
  * 現在起動しているディスクに直接インストールすることはできません。
* シングルブートでインストールする場合、ターゲットはパーティションではありません。この方法はディスク全体にインストールします。
  
  ***
  
### ブランチのインストール
  
3. ターゲットディスクが決まったら、Brunchのインストールは完了です。
  * `disk`をターゲットディスクに置き換えてください。 (例えば `sdb`、`mmcblk0`、`nvme0n1` など）。
  
```sudo chromeos-install -dst /dev/disk```
  
スクリプトが確認を求める。 インストールする準備ができたら、プロンプトに `yes` と入力してください。
  
ターゲットディスクの速度によってはインストールに時間がかかる場合があります。 GPTヘッダーのエラーがいくつか出るかもしれませんが、無視してください。
  
インストールが完了すると、ChromeOSがインストールされたことが報告されます。 ターミナルを閉じる前に、ターミナルに追加のエラーがないことを確認してください。 エラーがなければ、問題ありません！

### 次のステップ
  
It is normal for the first boot to take a very long time, please be patient.

* 最初の起動は、[changing kernels][change-kernels]や[framework options][framework-options]などの重要な設定をするのに最適なタイミングです。
* もし何か問題があれば、[Brunch Configuration Menu][edit-brunch-config] でパッチや解決策を確認することを強くお勧めします。
  
</details>  
  
  ***
 
# デュアルブートのインストール
このガイドはWindows WSL2を使ってBrunchをパーティションにインストールするためのものです。

<details>
  <summary>クリックしてデュアルブートガイドを開く</summary>

### 必要条件
- 管理者権限。
- ターゲットパーティションは16GB以上で、暗号化されておらず（ビットロッカーは無効）、NTFSでフォーマットされていること。
- WSL2がインストールされていること。
- `pv`、`tar`、`unzip`、`cgpt` パッケージ。
- Brunchを起動するための[互換PC][compatibility]。
- Linux ターミナルの入門レベルの理解。
  - このガイドはこのプロセスをできるだけ簡単にすることを目的としているが、基本的なことは知っていることが望ましい。
### リカバリー
1. お使いのCPUに適したリカバリーをダウンロードします。 以下のリストを参考にしてください。 最新のBrunchリリース番号と一致するリカバリーを選択する必要はありません。
  
#### Intel
* 第8世代と第9世代： Intel 用 "[shyvana][recovery-shyvana]"／Celeron 用 "[bobba][recovery-bobba]".
* 第 10 世代： 第 10 世代: "[jinlon][recovery-jinlon]".
* 第11世代以上： "[voxel][recovery-voxel]".
#### AMD
* Ryzen: "[gumboz][recovery-gumboz]".

リカバリーは上記のリンクをクリックすることで見つけることができます。 また、[cros.tech][cros-tech]にアクセスし、ご希望のリカバリーを検索することでも見つけることができます。

ご希望のリカバリーを選択した後、特定のリリースを選択することができます。 投稿されたリリースは現在のリリースより遅れているかもしれませんが、これは通常のことで、後で現在のリリースにアップデートすることができます。 通常、利用可能な最新のリリースを使用することをお勧めします。

### ファイルの収集
2. このGitHubリポジトリからBrunchのファイルをダウンロードしてください。他のサイトにあるファイルやオンライン上の動画にリンクされているファイルは使用しないでください。[releases tab][releases-tab]はGitHubのメインページの右側の列の一番下にありますが、一般的には[latest release][latest-release]を使うことが推奨されています。

リリースをダウンロードする際は、リリース記事の下にあるアセットからbrunch...tar.gzファイルを選択してください。 ソースコードファイルは必要ありませんので、ダウンロードしないでください。

続行する前に、WSL2を使ってマイクロソフトストアからLinuxディストロをインストールし、ディストロをセットアップして使用できる状態にしておく必要があります。 システムによってはセットアップが複雑な場合があるので、オンラインリソースを参照してください。

### ターミナルの準備
3. Brunchリリースと選択したChromeOSリカバリーの両方のファイルをダウンロードしたら、WSL2を起動します。
4. pv、cgpt、tar、unzipがインストールされていることを確認する。

```sudo apt update && sudo apt -y install pv cgpt tar unzip```
  * 私の例では、Debian と Ubuntu ベースのディストロのパッケージマネージャである `apt` を使っています。Arch を使っている場合は、cgpt にアクセスするために [vboot-utils][vboot-utils] が必要で、それ以外をインストールするには別のパッケージマネージャーが必要になるかもしれません。

4b. Linux のリリースによっては、上記の依存関係をインストールするために `universe` リポジトリが必要になる場合があります。 もし、依存関係が利用できないというエラーが表示された場合は、このコマンドで `universe` レポジトリを追加し、その後でもう一度前のステップを試してください。

```sudo add-apt-repository universe```
  
5. すべての依存関係がインストールされたら、ファイルをダウンロードしたディレクトリに `cd` します。
  * `username`をあなたの*Windows*ユーザー名に置き換えてください。
  * linuxのターミナルは大文字小文字を区別します。

```cd /mnt/c/Users/username/Downloads```
  
6. `tar` を使ってBrunchアーカイブを取り出す。
  * `brunch_filename.tar.gz` を実際のファイル名に置き換える。

```tar zxvf brunch_filename.tar.gz```
  
7. `unzip`を使用してChromeOSリカバリを解凍する。
  * `chromeos_filename.bin.zip`を実際のファイル名に置き換えてください。

```unzip chromeos_filename.bin.zip```

完了すると、ブランチアーカイブから4つの新しいファイルと、次のステップで使用するリカバリビンができます。

### ブランチのインストール

8. ファイルの準備ができたら、Brunchのインストールは完了です。
  * 先ほどと同じように、`chromeos_filename.bin`をbinファイルの実際のファイル名に置き換えてください。
  * また、`size` を整数で置き換える。 (例えば `14`、`20`、`100` など）。
    * また、`14`、`20`、`100` などの整数で置き換えることになります。

例えばBrunchをインストールするディレクトリを作る：
  - brunch を C: パーティションのホームフォルダにインストールしたい場合は、`mkdir /mnt/c/Users/username/brunch` を実行してください。
  - D: パーティションにインストールする場合は `mkdir /mnt/d/brunch` を実行してください。

  次に、"-dst "のあとに作成するイメージファイル名（ブランチディレクトリ）を指定してインストーラーを起動します。:
```sudo bash chromeos-install.sh -src chromeos_filename.bin -dst /mnt/c/Users/username/brunch/chromeos.img -s size```

ターゲットディスクの速度によってはインストールに時間がかかる場合があります。 GPTヘッダーのエラーがいくつか出るかもしれませんが、これは無視してかまいません。 インストールに十分な空き容量がないと言われた場合は、コマンドの末尾の数字を収まるまで減らしてください。 imgがパーティションの全領域を占有できないのは普通のことです。

インストーラーがインストールの種類を聞いてきたら、ターミナルに「dualboot」と入力し、「Enter」キーを押して続行する。

インストールが完了すると、ChromeOSがインストールされたことが報告されます。 続行する前に、ターミナルに追加のエラーがないことを確認してください。 エラーがなければ、続行しても問題ありません！
  
### Grub2Winのセットアップ
10. Grub2win][grub2win]をインストールし、プログラムを起動する。 (Windows DefenderがGrub2Winをウイルスと判断して削除することがあります。）

11. Manage Boot Menu`ボタンをクリックし、'Import Configuration File'で`Chrome`をクリックします。

  * 先ほど作成したchromeos.img.grub.txtファイルを選択します。
  * 選択した項目のインポート`をクリックします。
    * Apply`をクリックしない限り、入力内容は保存されません。

### WindowsがNTFSパーティションをロックしないようにする
12. 暗号化/休止状態を無効にする。

以下の操作を行わないと、ChromeOS は起動できず、安定しません (必要に応じて Windows オンライン リソースを参照してください)：
  - ChromeOS イメージが保存されているドライブで bitlocker が無効になっていることを確認するか、無効にします。
  - 高速スタートアップを無効にする。
  - ハイバネーションを無効にする。
  
この時点で、再起動する準備ができ、Windowsに起動する代わりにGrub2winメニューが出迎える。

### 次のステップ

最初のブートには非常に時間がかかるのが普通です。

* 最初の起動は、[カーネルの変更][change-kernels]や[フレームワークオプション][framework-options]などの重要な設定をするのに最適なタイミングです。
* もし何か問題があれば、[ブランチ設定メニュー][edit-brunch-config] でパッチや解決策を確認することを強くお勧めします。

  </details>
 
 ***
 
# [Troubleshooting and Support][troubleshooting-and-faqs]

問題がある場合は、[Troubleshooting and Support][troubleshooting-and-faqs] ページを参照してください。

### その他のヒント
* ブランチUSBの起動に問題がある場合は、BIOSでUEFIが有効になっていることを確認してください。
* PCによっては、USBからブートする際にキーを押すか、BIOSでUSBブートが有効になっている必要があります。
* ハードウェアによっては、最初の起動に1時間かかることがあります。 ブランチは通常、ブランチのロゴでフリーズすることはありません。 Brunchのロゴが表示されている場合、システムはまだ起動している可能性があります。
* PCがChromeOSのロゴ（背景は白）で止まっている場合、互換性のない専用GPUを使用している可能性があります。
* 検証失敗」というブルースクリーンが表示される場合は、BIOS設定でセキュアブートを無効にするか、[セキュアブートキーを登録][secure-boot]してください。
  * USBから直接キーを登録するには、[OK] -> [Enroll key from disk] -> [EFI-SYSTEM] -> [brunch.der] -> [Continue and reboot]を選択します。
* 正常なブート時にシステムが勝手に再起動する場合は、Brunchがエラーを起こしているため、高度なトラブルシューティングが必要です。

*** www.DeepL.com/Translator（無料版）で翻訳 ***


Brunchのインストールや使用中に問題が発生した場合は、Discordでサポートを受けることができます：

[![Discord][discord-shield]][discord-url]

<!-- Alternate Guide -->
## Linuxガイドをお探しですか？
### [![Linuxでインストール]][linux-img]][linux-guide] [Linuxでインストール][Linux-guide]]

<!-- Reference Links -->
<!-- Badges -->
[license-shield]: https://img.shields.io/github/license/sebanc/brunch?label=License&logo=Github&style=flat-square
[license-url]: ../LICENSE
[forks-shield]: https://img.shields.io/github/forks/sebanc/brunch?label=Forks&logo=Github&style=flat-square
[forks-url]: https://github.com/sebanc/brunch/fork
[stars-shield]: https://img.shields.io/github/stars/sebanc/brunch?label=Stars&logo=Github&style=flat-square
[stars-url]: https://github.com/sebanc/brunch/stargazers
[issues-shield]: https://img.shields.io/github/issues/sebanc/brunch?label=Issues&logo=Github&style=flat-square
[issues-url]: https://github.com/sebanc/brunch/issues
[pulls-shield]: https://img.shields.io/github/issues-pr/sebanc/brunch?label=Pull%20Requests&logo=Github&style=flat-square
[pulls-url]: https://github.com/sebanc/brunch/pulls
[discord-shield]: https://img.shields.io/badge/Discord-Join-7289da?style=flat-square&logo=discord&logoColor=%23FFFFFF
[discord-url]: https://discord.gg/x2EgK2M

<!-- Outbound Links -->
[croissant]: https://github.com/imperador/chromefy
[swtpm]: https://github.com/stefanberger/swtpm
[linux-surface]: https://github.com/linux-surface/linux-surface
[chromebrew]: https://github.com/skycocker/chromebrew
[intel-cpus]: https://en.wikipedia.org/wiki/Intel_Core
[intel-list]: https://en.wikipedia.org/wiki/List_of_Intel_CPU_microarchitectures
[atom-cpus]: https://en.wikipedia.org/wiki/Intel_Atom
[atom-list]: https://en.wikipedia.org/wiki/List_of_Intel_Atom_microprocessors
[amd-sr-list]: https://en.wikipedia.org/wiki/List_of_AMD_accelerated_processing_units#%22Stoney_Ridge%22_(2016)
[amd-ry-list]: https://en.wikipedia.org/wiki/List_of_AMD_Ryzen_processors
[recovery-bobba]: https://cros.tech/device/bobba
[recovery-shyvana]: https://cros.tech/device/shyvana
[recovery-jinlon]: https://cros.tech/device/jinlon
[recovery-voxel]: https://cros.tech/device/voxel
[recovery-gumboz]: https://cros.tech/device/gumboz
[cros-tech]: https://cros.tech/
[cros-official]: https://cros-updates-serving.appspot.com/
[vboot-utils]: https://aur.archlinux.org/packages/vboot-utils
[rufus-link]: https://rufus.ie/
[etcher-link]: https://www.balena.io/etcher/
[grub2win]: https://sourceforge.net/projects/grub2win/

<!-- Images -->
[decon-icon-24]: ../Images/decon_icon-24.png
[decon-icon-512]: ../Images/decon_icon-512.png
[terminal-icon-24]: ../Images/terminal_icon-24.png
[terminal-icon-512]: ../Images/terminal_icon-512.png
[settings-icon-512]: ../Images/settings_icon-512.png
[windows-img]: https://img.icons8.com/color/24/000000/windows-10.png
[linux-img]: https://img.icons8.com/color/24/000000/linux--v1.png

<!-- Internal Links -->
[windows-guide]: ./install-with-windows.md
[linux-guide]: ./install-with-linux.md
[troubleshooting-and-faqs]: ./troubleshooting-and-faqs.md
[compatibility]: ../README.md#supported-hardware
[changing-kernels]: ./troubleshooting-and-faqs.md#kernels
[framework-options]: ./troubleshooting-and-faqs.md#framework-options
[releases-tab]: https://github.com/sebanc/brunch/releases
[latest-release]: https://github.com/sebanc/brunch/releases/latest
[brunch-der]: https://github.com/sebanc/brunch/raw/main/brunch.der
[secure-boot]: ./install-with-linux.md#secure-boot
[brunch-usb-guide-win]:  ./install-with-windows.md#usb-installations
[brunch-usb-guide-lin]:  ./install-with-linux.md#usb-installations
[edit-brunch-config]: ./troubleshooting-and-faqs.md#brunch-configuration-menu
