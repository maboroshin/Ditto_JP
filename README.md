# Ditto 日本語化と使い方
オープンソースの**クリップボード履歴ソフト**の [**Ditto**](https://ditto-cp.sourceforge.io/) の高度な使い方の解説です。Ctrl 2回での呼び出し、正規表現置換、特定のプログラム上でのコピーではクリップ保存を除外したい、を解説。Dittoではダークモードがあり、ポータブル化で使えます。ポータブル化はほかのソフトにも多い。

この種のソフトは日本産にも秀逸な同種のソフトが多くあります（Clibor、ToMoClipなど）。オープンソースで様々なOSに対応したものにはCopyQもあります（2022年に翻訳調整しておきました）。

Ditto日本語化ファイルの公開は本家に組み込まれたので終了です。（2019年の Ditto バージョン 3.22.70 の日本語化ファイル）

## 特徴
- 指定日数で期限切れさせ履歴から消去することができる
- ショートカットキーのカスタマイズが豊富
- グループ化可能
- ポータブル版もある
- 文字列を変換して貼り付け (英語大文字小文字など)
- 履歴を [WinMerge](https://winmergejp.bitbucket.io/) で比較。ポータブルでは設定の「高度」(上級、Advanced) からWinMergeのパスを指定（PortableAppsのWinMergeでも可）。
- IPやコンピュータ名などを指定して、クリップボード内容を暗号化して送信
- ダークモード (2019年ベータ版 v3.22.70 にある機能。Windowsの設定に従うため、[Auto Dark Mode](https://github.com/Armin2208/Windows-Auto-Night-Mode)で自動変更できる)

- 履歴は1000件？
- "" で囲まない半角空白は AND 検索になる（その単語が両方ヒットするものを探す）。囲むと、その文字列通りに探す。
- 指定のソフトでは記録を除外。
- 正規表現検索 - 検索オプションから
- 正規表現置換 - [2019年ベータ版に追加されている](https://sourceforge.net/p/ditto-cp/ditto/ci/0fbe9a6625b45dd20ac5ad27d93998bc2c8f86d5/)。掲示板での[使い方](https://sourceforge.net/p/ditto-cp/discussion/287510/thread/2e47593585/)を見るとオプションの「高度」の中。下記で説明。

- [英語の公式説明 Wiki](https://github.com/sabrogden/Ditto/wiki)
- []()

## Ctrl 2回での呼び出し
日本のクリップボードソフトに多い、Ctrl での呼び出しには[AutoHotkey](https://autohotkey.com/download/) を使います。インストーラーもありますが、ここからzip版をダウンロードし解凍、以下の `.ahk` ファイルを、 `AutoHotkeyU64.exe` へとドラッグ&ドロップします。あとは Ctrl2回で Ditto を呼び出せます。

[DittoCall.ahk](DittoCall.ahk) :ダウンロードするならここ

`DittoCall.ahk` : の中身の解説（Dittoのパスは適宜書き変え） : 
```
Ctrl::

    KeyWait,Ctrl
    KeyWait,Ctrl,D T0.3
    If !ErrorLevel
        Run C:\program files\Ditto\Ditto.exe /Open

Return
```

## 各種使用法
分かりにくい設定を解説。

### 最初の10個のクリップの左側の番号数が小さい場合
「オプション」「高度」（以前は「上級」と表示）の `First Ten Hotkeys Font Size`

`9` から `11` くらいが無難かと。

### 特定のプログラム上でのコピーではクリップ保存を除外したい
同じく「高度」のスクロールして下の方の `Exclude clips by Regular Expressions` を展開。

* `RegEx` に除外したい文字列の正規表現。例: `(.|[\r\n])*`
* `Process Name` に適用したいプログラム名をカンマ区切り。空欄ではすべて。例: `Program.exe;Player.exe`

この機能はよく分かってなくて、適当なので正規表現を改良してください。[本家のWiki](https://sourceforge.net/p/ditto-cp/wiki/Excluding%20clip%20from%20saving/)には、こういう書き方がありました。

* `.*test.*` 本家に書いてあるやつ。（注釈：testを含む文字列すべてかと。 `.` が何らかの文字の意味で、`*` がその繰り返しを意味かと。たぶん）
* `.*` なのでこうしてみた。空白を含むあらゆる文字列に合致しているが、改行が入るとだめだ。
* `(.|[\r\n])*` 改行を含めてみた。合格？上に書いたやつと同じ。
* `\[\w\sA-Za-z0-9@#$%^:/&.\]+` うまくいかなかったやつ。

### 正規表現による置換
同じく「高度」のボタン `On Paste Script` で
1. 「Add」を押す。
2. Nameを適当につける。この名前で「変換して貼り付け」（special paste）に追加される。
3. Script に記入。以下にサンプルコード。
4. Sample Input にテスト用文字列を入れ Run で、 Output にテスト結果が出るのでこれでデバッグしてスクリプトを作る。

Script のサンプルコード:
```
clip.AsciiTextReplaceRegex("([a-z])", "J$1");
clip.AsciiTextReplaceRegex("[a]", "A");

return false;
```

正規表現には、一部エスケープが必要。
* \\\\s 空白
* \\\\d 数字
* [any] - 通常と同じ
* () と $1..$2.. - 通常と同じ

## 翻訳メモ
- 言語ファイルの、開発者による変更を追うには [Englishe.xml](https://github.com/sabrogden/Ditto/blob/master/Debug/Language/English.xml) のHistory。
- ないリソースの指定番号は、[CP_Main.rc](
https://github.com/sabrogden/Ditto/blob/master/CP_Main.rc) で文字列を検索。その文字列に当てられた変数名（大文字）を、[Resource.h](https://github.com/sabrogden/Ditto/blob/master/Resource.h) 検索し、その割り当ての番号を使ってみる。
- 親のタグは [MultiLanguage.cpp](https://github.com/sabrogden/Ditto/blob/master/MultiLanguage.cpp) に書いてある。

### Waiting for multilingualization
- "Save/move to group" / "Hotkey available globally" (Copy Properties Dialog)
- Context Menu: Ditto Utils: All below...
    - "Paste any clip as text" (Ditto Utils)
    - "Paste as html image link" (Ditto Utils)
    - "Clear read only flag" (Ditto Utils)
    - "Set read only flag" (Ditto Utils)
- "Send To Friend" Dialog:All: IP or Host Name ... et.
    - "Error Sending data to %s"
    - "ERROR opening connection to %s"
- Advanced Options (All)
- Options: Theme: Classic / DarkerDitto (ここは残っててもいいかも)

## 翻訳ファイルの導入方法（2019年に翻訳ファイル本家組み込み前の情報）
ベータ版をダウンロードして使うのが早いです。PortableAppsの場合、照合するフォルダにベータ版を上書きするといいでしょう

以下は手動で言語ファイルを入れる場合です。

1. `Language` に Japanese.xml を入れる。（ PotableApps なら `DittoPortable\App\Ditto\Language`） 
2. Option - General - Language から表示言語を変更。
3. 最初操作に慣れるために、 Ditto の見出しをダブルクリックで「常に手前に表示」もできます。
4. ショートカットキーの設定で Ditto の呼び出しを  `Ctrl + 3` とかにすれば呼び出しやすいのでは。

- 2019-11-06 翻訳、Rev1 (※)
- 2019-11-07 Rev2、プログラムコード上で多言語化されていないもの以外は翻訳。
- 2019-11-20 Rev3、ショートカットキーを追加。
※ベータ版のv3.22.70 (2019/10/28) をベースに翻訳したら、正式版のv3.22.20 (2018-12-23) では、メニューの起点が未翻訳になりました。この正式版に整合するのは手間なのでそのままです。（古い2018年のバージョンを使う場合の説明）

### Credit
Ditto is published under license GPL3. (Outdated information since translation files were incorporated)

- Original File is  [Japanese.xml](https://github.com/sabrogden/Ditto/blob/master/Debug/Language/Japanese.xml) by(ScottBrogden, Nardog).

