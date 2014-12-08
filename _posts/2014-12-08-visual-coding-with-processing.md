---
layout: post
title: Processingで始めるメディアアート入門
date: 2014-12-08 0:00
---

Processingで始めるメディアアート入門

この記事は[LiT!関西 Advent Calendar 2014](http://www.adventar.org/calendars/607) 8日目の記事です。
LiT(Life is Tech!)とは中学生、高校生のためのプログラミング・ITキャンプ／スクールのことであり、このアドベントカレンダーは、LiT所属の大学生メンターが書いています。

すこし大げさなタイトルですが、この記事は、メディアアートってなんとなくおもしろそう、なのでメディアアートやってみたい。

けどどうやって始めればいいのか分からない。

そんな人はProcessingがいいよ！！！という記事です。

## メディアアートってなんだ

一重にメディアアートと言ってもその内容は多岐にわたり、一言では言えないのですが、wikipediaによると、

> 20世紀中盤より広く知られるようになった、芸術表現に新しい技術的発明を利用する、もしくは新たな技術的発明によって生み出される芸術の総称的な用語である。
> http://ja.wikipedia.org/wiki/%E3%83%A1%E3%83%87%E3%82%A3%E3%82%A2%E3%82%A2%E3%83%BC%E3%83%88

とのこと。

最近、メディアアート、という言葉を、プロジェクションマッピングなどの流行もあって聞くことが多いような気がしていて、LiTでも、メディアアートコースというものがあることもあり、ボクも含め興味を持っている人は多いと思う。

今回はプログラミングをつかったものを紹介する。
ビジュアルコーディングというやつ。


## Processing

そこでProcessingの登場です。僕はこれからやってみたいという人にはprocessingを勧めたいかなと思ってる。

理由としては

- 文法が簡単
- 直感的に書ける
- すぐに結果が見れる

ことが大きい。

### 文法が簡単

メディアアートを体験したいわけであって、言語の勉強をしたいわけではない。
ましては入門したいときに複雑な文法など覚えてられない。最低限にしたい。

そういう意味でoF(C++)はあんまり入門向けではないと思った。oFも試すにはそれほど文法を知る必要はないんだけど、全体が掴めない。


### 直感的に書ける。

線引きたい、とおもったら

{% highlight JavaScript %}
line(x1, y1, x2, y2);
{% endhighlight %}

これでよい

0から10の範囲で乱数が欲しいなら

{% highlight JavaScript %}
ramdom(10)
{% endhighlight %}

これでよい。余計なプレフィックスもつかない(大事)
もしこれが最初に学ぶ言語だったとしても、「何をすれば何ができるのか」がコードから分かりやすい。

これはメディアアートに必要な知識として、基本的な数学があることにも関連していて、文法が分かりやすいことで数学が苦手な人でも理解するのが少しでも楽になれるのではないかと思っている。


### すぐに結果が見れる

これが一番大きい。

元はJavaなので実行してwindowが表示される感じだけど、
JavaScriptに変換する機能を簡単にインストールでき、変換したものをそのままサーバーなどに上げればだれでもすぐに結果を見ることができる。


## 試してみる

Processingは [https://processing.org/download/?processing](https://processing.org/download/?processing) からダウンロードできる。

Mac, Windows, Linuxそれぞれに対応しているので、各々の環境に合わせてインストールしてください。


インストール後、このようなwindowが開く

![{{ site.url }}/image/processing1.png]({{ site.url }}/image/processing1.png)

ここに書いて、左上の実行ボタン（▶）を押すと別ウィンドウで結果が表示される。

試しに書いてみる

{% highlight JavaScript %}
// 座標をきめる
float x = width / 2;
float y = height / 2;
// 円を書く
ellipse(x, y, width, height);
{% endhighlight %}

これを書いて実行すると

![{{ site.url }}/image/processing2.png]({{ site.url }}/image/processing2.png)

こうなる。メディアアート！！！！！！！！！！！

もう少し実践的にやってみる。

Processingは`setup()`と`draw()`というメソッドをそれぞれ定義することで、それらがそれぞれが初期化処理と描画処理を担当する。

{% highlight JavaScript %}

int start = 0;
int limit = 100;
float x;
float y;
float w,h;
boolean incr = true;

void setup() {
  size(500, 500);
  x = width / 2;
  y = height / 2;
  w = start;
  h = start;
}

void draw() {
  background(255);
  if(w > limit){incr = false;}
  if(w < start){incr = true;}
  if(incr){
    w++;
    h++;
  }else{
    w--;
    h--;
  }
  ellipse(x, y, w, h);
}

{% endhighlight %}¥

円を大きくしたり小さくしたりする。
少しながいけど、Processingで書くとシンプルで分かりやすいのではないかと思う。






