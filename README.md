# Michao SSP 統合ガイド

Michao SSPは3つの広告フォーマットをサポートしています。リワード広告、バナー広告、ワイプアドです。以下ではそれぞれの広告フォーマットの概要と実装方法を説明します。

- [接続情報について](#connection)
- [バナー広告](#banner-ad)
- [ワイプ広告](#wipe-ad)
- [リワード広告](#rewarded-ad)

## 接続情報について  <a name="connection"></a>

Michao!!と接続した際にはメールでコードとplacement_idが送られてきますが、placement_idはリワード広告のみで使用されます。また、配布されたコードの一つ目のScriptタグはheadに埋め込んでください。

一つ目のScriptタグ: ```<script async src="https://serving.lookverin.com/player/adlook.js"></script>```

## バナー広告 <a name="banner-ad"></a>

- 対応アスペクト比：16:9、4:3、1:1、9:16
- 呼び出し方法：指定した要素内に以下のコードを埋め込む

```
<div class="banner">
  <script data-playerPro="current">(function(){var s=document.querySelector('script[data-playerPro="current"]');s.removeAttribute("data-playerPro");(playerPro=window.playerPro||[]).push({id:"placement_id",after:s});})();</script>
</div>
```

- 複数のバナー広告を呼び出す場合、同じコードを複数の要素に埋め込むことができます。
- 画面外に出た際の挙動をカスタマイズし、スティッキー広告を設定することも可能です。

### バナー広告 例

## ワイプアド <a name="wipe-ad"></a>

- 実装方法: body内の好きな位置に配布コードを埋め込む

```
<body>
  <div class="container">///</div>
  <script data-playerPro="current">(function(){var s=document.querySelector('script[data-playerPro="current"]');s.removeAttribute("data-playerPro");(playerPro=window.playerPro||[]).push({id:"placement_id",after:s});})();</script>
</body>
```

### ワイプアド 例

## リワード広告 <a name="rewarded-ad"></a>

- リワード広告は通常、ページ読み込み時に再生するか、広告再生ボタンで呼び出すかの二つの動作が想定されます。

- ページ読み込み時にコンテンツを隠すように再生する方法は、ワイプアドと同じようにリワード広告を仕掛けたいページの好きな位置に配布コードを設置してください。

```
<body>
  <div class="container">///</div>
  <script data-playerPro="current">(function(){var s=document.querySelector('script[data-playerPro="current"]');s.removeAttribute("data-playerPro");(playerPro=window.playerPro||[]).push({id:"placement_id",after:s});})();</script>
</body>
```

### [ページ読み込み時に発火するリワード広告の例](examples/rewarded.html)


### 広告再生ボタンを用意し、動画再生終了やスキップなどの報酬を受け取れるタイミングでページ遷移またはポイント付与などの報酬獲得の処理を行いたい場合は、以下を参照してください。

- 呼び出し方法: 広告再生ボタンのクリックなどのタイミングで以下のコードを発火

```(playerPro=window.playerPro||[]).push({id:"placement_id",after:s});})();```

- 動画広告の再生終了時やスキップ時にコールバックを設定可能です。以下は例です。

```
(playerPro=window.playerPro||[]).push({
  id:"placement_id", // プレースメントID
  after:s,  
  init: (api) => {
    api.on('AdVideoComplete', function(){
      // 動画広告再生終了時の処理
      // 報酬処理（例：ページ遷移やポイント付与）
    });
    api.on('AdVideoSkip', function(){
      // 広告スキップ時の処理
      // 報酬処理（例：ページ遷移やポイント付与）
    });
  }
});
```

### 任意のタイミングで再生するリワード広告の例

- [例](examples/rewarded_2.html)

リワード広告にはほかにも様々な再生開始や広告クリックなどのコールバック(イベント)が用意されています。詳しくはWikiをご覧ください。






