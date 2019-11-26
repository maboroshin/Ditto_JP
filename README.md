# Ditto 3.22.70 日本語化
オープンソースのクリップボード履歴ソフトの [Ditto](https://ditto-cp.sourceforge.io/) の日本語化です。本家のベータ版に既に組み込まれたのでここでの公開は停止します。ベータ版のv3.22.70 (2019/10/28) をベースに翻訳したら、正式版のv3.22.20 (2018-12-23) では、メニューの起点が未翻訳になりました。整合するのは手間なのでそのままです。

- 2019-11-06 翻訳、Rev1
- 2019-11-07 Rev2、プログラムコード上で多言語化されていないもの以外は翻訳。
- 2019-11-20 Rev3、ショートカットキーを追加。

## 特徴
- 指定日数で期限切れさせ履歴から消去することができる
- ショートカットキーのカスタマイズが豊富
- ポータブル版もある
- 文字列を変換して貼り付け (英語大文字小文字など)
- 履歴を [WinMerge](https://winmergejp.bitbucket.io/) で比較。ポータブルでは設定の「上級」(Advanced) からWinMergeのパスを指定（PortableAppsのWinMergeでも可）。
- IPやコンピュータ名などを指定して、クリップボード内容を暗号化して送信
- ダークモード (2019年ベータ版 v3.22.70 にある機能。Windowsの設定に従うため、[Auto Dark Mode](https://github.com/Armin2208/Windows-Auto-Night-Mode)で自動変更できる)

- 履歴は1000件？
- "" で囲まない半角空白は AND 検索になる（その単語が両方ヒットするものを探す）。囲むと、その文字列通りに探す。
- 正規表現検索 - 検索オプションから
- 正規表現置換 - [2019年ベータ版に追加されている](https://sourceforge.net/p/ditto-cp/ditto/ci/0fbe9a6625b45dd20ac5ad27d93998bc2c8f86d5/)けど呼び出し方が不明。誰かSourceforgeで聞いてみて。

## 導入方法
ベータ版をダウンロードして使うのが早いです。PortableAppsの場合、照合するフォルダにベータ版を上書きするといいでしょう

以下は手動で言語ファイルを入れる場合です。

1. `Language` に Japanese.xml を入れる。（ PotableApps なら `DittoPortable\App\Ditto\Language`） 
2. Option - General - Language から表示言語を変更。
3. 最初操作に慣れるために、 Ditto の見出しをダブルクリックで「常に手前に表示」もできます。
4. ショートカットキーの設定で Ditto の呼び出しを  `Ctrl + 3` とかにすれば呼び出しやすいのでは。

## Ctrl 2回での呼び出し
日本で多いCtrl呼び出しには[AutoHotkey](https://autohotkey.com/download/) を使います。インストーラーもありますが、ここからzip版をダウンロードし解凍、以下の `.ahk` ファイル作り、 `AutoHotkeyU64.exe` へとドラッグ&ドロップします。あとは Ctrl2回で Ditto を呼び出せます。

`DittoCall.ahk` : その中身（Dittoのパスは適宜書き変え） : 
```
Ctrl::

    KeyWait,Ctrl
    KeyWait,Ctrl,D T0.3
    If !ErrorLevel
        Run C:\program files\Ditto\Ditto.exe /Open

Return
```

## Credit
Ditto is published under license GPL3.

- Original File is  [Japanese.xml](https://sourceforge.net/p/ditto-cp/ditto/ci/master/tree/Debug/Language/Japanese.xml) by(ScottBrogden, Nardog).

## 翻訳メモ
- 言語ファイルの、開発者による変更を追うには [Englishe.xml](https://sourceforge.net/p/ditto-cp/ditto/ci/master/tree/Debug/Language/English.xml) のHistory。
- ないリソースの指定番号は、[Resource.h](https://sourceforge.net/p/ditto-cp/ditto/ci/master/tree/Resource.h) で文字列を検索。その文字列に当てられた変数名（大文字）を、[CP_Main.rc](
https://sourceforge.net/p/ditto-cp/ditto/ci/master/tree/CP_Main.rc) 検索し、その割り当ての番号を使ってみる。
- 親のタグは [MultiLanguage.cpp](https://sourceforge.net/p/ditto-cp/ditto/ci/master/tree/MultiLanguage.cpp) に書いてある。

### Waiting for multilingualization
- Context Menu: Ditto Utils: All below...
- "Send To Friend" Dialog:  IP or Host Name ... et.
- Options: Theme: Classic / DarkerDitto
- Optiions: Position (With variables): At caret / At cursor / At previous position
- Advanced Options