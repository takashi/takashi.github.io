---
layout: post
title: GraftJs/graft
date: 2014-10-26 1:51
---

進捗を作っていく

煩雑なメモ

### graft

node.jsでmicroservicesを作るためのフレームワーク

service間の通信にはlibchanのjs実装のjschanを使っている
libchanはgolangのchannelをもっと汎用的に使えるようにしたもの

説明は [graft founderのプレゼンがある](http://mcollina.github.io/nodeconfeu-2014-full-stack-through-microservices/ )


以下自分なりのまとめ

`.pipe(service).pipe(another)` がやりたいこと？

service同士をpipeでつなぐ(stream api)

一番最初のアイデアはRemote Procedure Call(RPC)
RPCが実現したかったのは位置透過性

ローカルの呼び出しも、リモートの呼び出しも同じメソッドコールで呼び出せる。

* 問題点としては
localは必ずつながるけど、remoteはそうではない、数百msの遅延が発生するし、必ずアクセスできるかもわからない

> 分散システム間で、メッセージ通信を行いながらアプリケーションが動作するための方法に遠隔手続き呼び出し（RPC）がある。RPCの特徴はメッセージ通信に手続き呼び出しという抽象化を与えて分散プログラミングを容易化している。また、異機種間通信を容易化する機能も備えている。
http://www.curri.miyakyo-u.ac.jp/curri-ex/os/txt/os7.html


nodeだと、Dnodeというものもあった

DnodeはnodeでRPCサーバを立てるためのライブラリ

```js
var dnode = require('dnode');
var server = dnode({
    replace: function (s, cb) {
        cb(s.replace(/[aeiou]{2,}/, 'oo').toUpperCase())
    }
});
server.listen(5004);
```

こんな感じでサーバを立てると、クライアントで

```js
//client.js
var dnode = require('dnode');

var d = dnode.connect(5004);
d.on('remote', function (remote) {
    remote.replace('syuta', function (res) {
     console.log(res)
        d.end();
    });
});
```

このようにremoteのメソッドを叩ける


### Takeaways

from RPC(?)

- request/response pattern
- built on streams
- JSON message format
- works from the browser, too!
- data must be ready before calling the RPC
- no live feed, 'no realtime'

from REST

- Most of the time not used under Roy's guidelines
- A much nicer version of RPC?
- No live feed (Server-Sent Events?)
- Is WebSocket REST?

このへんよくわからない、影響をうけたもの？


### LibChan

- Built in Go
- future basis of Docker
- SPDY all the things!
- Like Go Channels over the Network
- MsgPack all the things!
- unidirectional


#### jsChan

> you manipulate channels via through each MicroService is just a through away SPDY, or Websocket

SPDYかWebSocketを通じて、channel使ってservice間の通信をする？

### SPDYとは？

SPDYでスピーディと読む
httpのRTT(Round Trip Time)の短縮を目指している

現在ブラウザ側ではchrome,firefox,operaのデスクトップ、モバイル版でSPDY対応が完了している。

> これはブラウザによるWebページの読み込み処理が、「DNSによるアドレス解決」、「TCP接続のハンドシェイク」、「HTTPのリクエストとレスポンス」といった３つのステップから構成されており、それぞれの処理時間が回線速度よりもRTTに大きく依存していることに起因しています。
http://www.iij.ad.jp/company/development/tech/activities/spdy/


サーバ側はapache, nginx, Jetty, node.js(!)

> 最近では、ブラウザ側のSPDY対応に合わせて、apache、nginx、JettyといったWebサーバ側でもSPDYがサポートされるようになりました。現在これらのWebサーバをSSLで使っているサイトであれば、誰でも独自にSPDYを利用することが可能です
http://www.iij.ad.jp/company/development/tech/activities/spdy/

技術的には「http接続を操作するsession層の置き換えを行ったもの」と定義できる。
→  OSI参照モデル！


利点として

- ブラウザへのWebサイトのページ読み込み時間を短縮する
- Webコンテンツの変更を必要としない
- 存のHTTPサーバと互換性を保ちつつ簡単に順次デプロイ(Drop-in replacement)が可能である






### graftメモ

`through.obj` の中にmicroserviceを実装する

足し算をするだけのservice(adder service)

```js
var through = require('through2');
module.exports = function build() {
  return through.obj(function(msg, enc, cb) {
    var result = msg.a + msg.b;
    msg.returnChannel.end(result);
    cb();
  });
}

// exposing it
if (require.main === module) {
  require('graft/spdy')
    .server({ port: 3001 })
    .on('ready', function() {
      console.log('Added listening on port', 3001);
    })
    .pipe(module.exports());
}

```

これをclientから遠隔で呼ぶ

```js
var graft = require('graft')();
var spdy  = require('graft/spdy');
var ret   = graft.ReadChannel();
graft.pipe(spdy.client({ port: 3001 }));
graft.write({
  a: 2,
  b: 2,
  returnChannel: ret
});
ret.on('data', console.log); // 4
```



こっからもうちょっとgraftのコード

小さいサンプル

```js
var graft = require('graft')();
var through = require('through2');


graft.pipe(through.obj(function(msg, enc, cb){
  // {hello: 'world'}
  console.log(msg);
  cb();
}))

graft.write({ hello: 'world' });

```



serverとclient

server

```js
var graft = require('graft')();
var spdy = require('graft/spdy');

graft.pipe(spdy.client({ port: 12345 }));

graft.write({ hello: 'world' });
```

client

```js
var graft = require('graft')();
var spdy = require('graft/spdy');
var through = require('through2');

spdy
  .server({ port: 12345 })
  .pipe(graft)
  .pipe(through.obj(function(msg, enc, cb) {
    console.log(msg); // prints { hello: 'world' }
    // process your request
    cb();
  }));
```


`.write`がchannelを通じてstreamにデータを送る?

