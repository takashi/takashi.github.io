---
layout: post-no-feature
title: 個人的におすすめなSublimeTextのプラグイン
categories: articles
---


[前回書いたSublime Textの記事](http://tak0303.github.com/2012/09/25/how-to-use-sublime.html)が結構いろんな方に読んでもらえて嬉しい限りです.

今回はSublime Textを使っていてボクが個人的におすすめするプラグインとその使用方法などを書いていきたいと思います.


## 1. Twitter Bootstrap Snippets


名前の通り,twitter bootstrapのコードを集めたsnippetです.

前回の記事でも紹介したのですが,個人的にかなり気に入っているので紹介します.

これだけで簡単なサイトのデザインが数行のsnippetのコードで作れてしまうので,お勧めです!

吐き出されたコードを見るだけでも参考になったりもするのではないでしょうか.

一つだけ注意したいのは,自動で吐き出されるtwitter bootstrapのjsのリンク(後でも説明します)のバージョンが少し古いこと.
最新のバージョンを使いたい場合は自分でリンクを書き換える必要があります.

インストールはpackage controlから twitter bootstrapで検索すれば出てきます.

snippetのコマンド一覧はコマンドパレット(cmd + shift + p)でbootstrapで検索すればみることができます.

こんな感じ

<img src="/images/sublime_6.png" alt="tbs">

一応,使えそうなのをいくつか紹介しておきます.
すべて入力後,tabキーでsnippetが展開されます


- tbnavbar

  よくあるナビゲーションバーを簡単に作成出来る. navbar-fixed-topのクラスだけ足してやると良いかも

- tbcarousel

  スライドショー. 画像はデフォルトでよくわからない画像が入っているのでお好みで.

- tbaccordion

  アコーディオン. クリックしたら下にスライドしてコンテンツが出てくるアレ.

- tbalert

  アラート表示. 汎用性が高いのでいろんなところに使えそう.tbnoticeとかもある.色が違う.

- tbhero

  [このページ](http://twitter.github.com/bootstrap/examples/hero.html)がそのまま作れる.用途はないかもだが,面白い



## 2.Gist

GistをSublimeTextから操作するプラグインです.

触ってみたら便利だったので.

インストールはおなじみpackage controlから.

インストールしたら、メニューバーから, preference　→　Package Settings →　Gist → Settings Userで Gist(Github)のユーザ名パスワードを入力. その他の項目は必須ではないので割愛させて頂きます.

操作は簡単 Gistに投稿したいファイルを開いて、コマンドパレットから.Gistを検索.

<img src="/images/sublime_7.png" alt="Gist">
(ボクのはGitが最初になってますが...)

Create Public(Private) Gistを選択,そしたらGistが作成されます.(作成された時,そこに自動で飛べばもっといいんですが...)

作成はこれだけ,簡単です!

もちろんいまあるGistの編集もできます.

コマンドパレットから Gist→ Open Gistで自分のGist一覧がでるので,そこから好きなものを選択,するとSublime上にGistが開けます.

あとはそれを好きに編集して、そのままUpdate Gist, Delete Gistなどが実行できます.

そして,別のファイル上にGistのコードを展開することもできます.

Gist →　Insert GistでGistを選択,するといま開いているファイルにGistのコードが挿入されます.

簡易スニペットのような形で使うこともでき,便利!



## 3.GoTo Documentation

php,javascript(coffeescript),python,Clojure,Go,Smarty,Ruby on Railsのドキュメントが検索できるプラグインです.

インストールはpackage controlから.

インストールしたらまずはキーバインドを設定します.

しなくてもいいのですが,ドキュメントは素早くみたいもの,設定することをおすすめします(しない場合はコマンドパレットから実行できます)

ボクは[githubのリポジトリ](https://github.com/kemayo/sublime-text-2-goto-documentation)にかいているキーバインドをそのまま使っています

設定は preference → KeyBindings Userに書きます.

```
{ "keys": ["super+shift+h"], "command": "goto_documentation" }
```

こんな感じ.

設定したら,使い方としては調べたい関数などを選択して,キーを押すだけ! そしたらそしたらそれぞれの言語のドキュメントサイト,選択した単語を検索した状態で飛びます.


こんな感じです.
まだ紹介できるものはあるのですが,[前回の記事](http://tak0303.github.com/2012/09/25/how-to-use-sublime.html)にもかいてあるので,そちらも参考にしていただけると嬉しいです!


## Tips:

### アイコン

ところで,SublimeTextのアイコンはどんなものかというと

<img src="/images/sublime_icon.png" alt="icon" width="100px">

こんな感じです.
そのままでもいいのですが,SublimeTextの前身,TextMate2のアイコンをみると..

<img src="/images/textmate.jpeg" alt="textmate" width="100px">

[こちらの方](http://ntcncp.blogspot.jp/2012/07/sublime-text2rails.html)もおっしゃっているんですが,完敗です.SublimeTextのアイコンダサい.

しかしダサいことはわかっていてもそう簡単に変えることなんて出来ない,画像がない...

そんなとき,ふとgithubを見ると,

<img src="/images/sublime_8.png" alt="kami" style="border:1px solid #ccc;">


....!?!?!?!?!?!?!?

なんと有志の方がアイコンを作ってました.Githubバンザイ

この方もデフォルトのアイコンが気に入らなさすぎて作ってしまった様子.

こちらからダウンロードして,その中の Sublime Text 2.icns がアイコン画像です.

これを

```
/Applications/Sublime\ Text\ 2.app/Contents/Resources/
```

にある Sublime Text 2.icns と入れ替えましょう.(元のアイコンはold_とかの名前で残しておくことをおすすめします!)
このとき名前が違わないか,しっかり確認しましょう.ボクは新しいアイコンの方の名前が　Sublime+Text+2.icns　このように + が入っていて最初うまく出来ませんでした.

こんな感じのアイコンになりました

<img src="/images/kami.png" alt="kami" width="100px">

おおお...かっこいい...

これでSublimeTextを使うモチベーションがさらに高まるかも? 公式の方もアイコンの改善(?)お願いします!






















