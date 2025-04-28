---
marp: true
theme: enuke
size: 16:9
paginate: true
html: true
---

<div class ="title">何をすれば良いデザインの Viz を</br>作れそうか調べてみた</div>

---

# 自己紹介

<div class="content-area">
    <div class="image-container">
                <img src="./images/0.jpg" class="circular-image" alt="Profile Image">
                <div class="image-name">nk(えぬけ〜)</div>
            </div>
    <ul>
        <li>業務:データ前処理,データ可視化,アプリケーション作成などなど</li>
        <li>普段はPowerBI→先日なんとかDATASaberになれました。</li>
    </ul>
</div>

---

<h1>DATASaber になった!🙌</h1>
<div class="content-area">
...</br>...</br>...</br>...</br>...</br>...</br>...
<h4>でもかっこいい Viz つくれないな...?</h4>
</div>

---

# 🤔

<div class="content-area">
<ul>
<li>これまで作ってきた Viz...デフォルトの機能しか使ってない</li>
<li>対して Tableau Public にあるのはとてもデフォルトの機能で</br>作ったものとは思えない...</li>
<li>どうやったら良いデザインの Viz を作れそうか"初めて"</br>調べてみた 🔨</li>
</ul>
</div>

---

# 1.Figma で公開されているテンプレートを使う

<div class="content-area">
<div class="center">
  <img src="./images/1.png" alt="画像" width="70%">
</div>
</div>
<style>
.center {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
}
</style>

---

# Web アプリのデザインのように編集 → Export

<div class="content-area">
<div class="split">
  <div><img src="./images/2.png" alt="左側の画像" width="100%"></div>
  <div><img src="./images/3.png" alt="右側の画像" width="100%"></div>
</div>

<style>
.split {
  display: flex;
  justify-content: space-between;
  gap: 20px
}
.split > div {
  width: 48%;
}
</style>
</div>

---

# テンプレート通りの Viz が作成

<div class="content-area">
<div class="split">
<img src="./images/4.png" alt="画像" style="width: 50%; height: auto;">
<img src="./images/5.png" alt="画像" style="width: 50%; height: auto;">
</div>

---

# しかし...

<div class="content-area">
<ul>
<li>無料版が 10 回までの Export...
<li>デザイン →Viz となるとデータ探索はしづらい気がする...
<li>今回は断念
</ul>
</div>

---

# 2.カスタムテーマを使用する方法

<div class="content-area">
<ul>
<li>Viz 作成はそのまま
<li>書式設定だけ json で作成できる
<li>json を一から書くのは...→ 便利なサイトが!
</ul>
</div>

---

# ブラウザ上で色合いを試せるサイトが!

<div class="content-area">
<div class="center">
<img src="./images/6.png" alt="画像" style="width: 70%; height: auto;">
</div>
</div>

---

# 自由に書式をカスタマイズして json を出力

<div class="content-area">
<div class="split">
<img src="./images/7.png" alt="画像" style="width: 50%; height: auto;">
<img src="./images/8.png" alt="画像" style="width: 50%; height: 100%">
</div>
</div>

---

# カスタムテーマをインポート!

<div class="content-area">
<div class="split">
<img src="./images/9.png" alt="画像" style="width: 50%; height: auto;">
<img src="./images/10.png" alt="画像" style="width: 50%; height: auto;">
</div>
</div>

---

# カスタムテーマは手頃に使えそう

<div class="content-area">
<ul>
<li>比較的色々な色合いを手軽に試せる。
<li>Viz のサイズなどは(おそらく)自分で変更
<li>簡易的な用途なら必要十分
</ul>
</div>

---

# 番外編:marp に埋め込む

<div class="content-area">
<div class="center">
Tableau に Web ページ埋め込み機能が...<br>→html に埋め込もう
<img src="./images/11.png" alt="画像" style="width: 50%; height: auto;">
</div>
</div>

---

# tableau を埋め込んだ html を iframe で marp に

<div class="iframe-container">
  <iframe src="https://nkwork9999.github.io/-LT-20240423tableau/" width="100%" height="400px"></iframe>
</div>

---

# グラスモーフィズムの CSS をあてる

<div class="iframe-container">
  <iframe src="https://nkwork9999.github.io/-LT-20240423tableau/" width="100%" height="400px"></iframe>
</div>

---

# まとめ

<div class="content-area">
<ul>
<li>簡単に使うなら「カスタムテーマ」
<li>html + css を使える状況なら埋め込みもあり?<br>そういう状況が果たしてあるか...
<li>もっと自由に CSS とかあてられたらな...
</ul>
</div>
