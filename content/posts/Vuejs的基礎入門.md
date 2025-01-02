+++
date = '2024-12-02T10:44:40+08:00'
draft = false
title = 'Vue-js的基礎入門'
tags = ['軟體教學','Vue.js']
+++

<img src="/images/article/about_vue.jpg" alt="Forest" width="600px">
<br>
<p style="color:"><strong>內容提及:</strong>Vue的常見指令、事件與迴圈</p>
<!--more-->

## 前情提要

一、v 開頭都是 Vue 的指令，value 只有 script 裡面要加。<br>
二、用 `ref` 包起來的這個變數，Vue 就可以知道變動，可以放字串、物件、陣列，且會反映在畫面上。<br>
三、不用關心 DOM，因可以直接去抓，可以不用使用到` document.queryselector`。<br>
四、會用兩個以上的元件做命名，是為了做區別
五、import 進來的子元件可以當 tag 使用
<a href="https://play.vuejs.org/#eNp9kctOwzAQRX9l8CYgVakQrEpa8VClwgIQILHxJkqnqYtjW36ESlH+nbFDUxaoiyiZudeTcz0duzMmbwOyGStcZYXx4NAHs+BKNEZbDx1Y3EAPG6sbyMiaccVVpZXz0Lga5lE/z1YopYZPbeX6LLu4OXqEMsGvSrWWaMmMLSoP8wV0XEEckLelDHhQcl/aGv3QpCk9PcV0ICMmKjw2RpYeqQIotpeLrkscfV9MqUrd9E+4Ta85Z38ROINpMsW54yw2Yd4R70bU+c5pRdeR+DirdGMEnXsxXlAezmYDedRKivz9lHreBpwc+tUWq69/+ju3jz3OXi06tC1yNmpD7kFevj/jnr5HsdHrIMl9QnxDp2WIjIPtPvzGHX2J9jEtVaj6wy33HpU7hIqg0dknP2e06IcT0Y+4V/l1Oke7Yv0PGrvFqQ==" target="_blank">練習網址</a><br>
練習一個 Hello World!

```html
<script setup>
  import { ref } from "vue";

  const msg = ref("Hello World!");
</script>

<template>
  <h1>{{ msg }}</h1>
  <input v-model="msg" />
</template>
```

## 事件

v-model ：這個指令專門用在表單模式上(input 裡面)<br>
contenteditable ：可以加在 div 裡面，讓它可以有個值

```html
<script setup>
  import { ref } from "vue";

  const msg = ref("Hello World!");
</script>

<template>
  <h1>{{ msg }}</h1>
  <div contenteditable="msg"></div>
</template>
```

v-on ＋事件 ：這個指令可以用＠來取代

```html
<script setup>
  import { ref } from "vue";

  const msg = ref("Hello World!");

  const inputHandler = (event) => {
    msg.value = event.target.value;
  };
</script>

<template>
  <h1>{{ msg }}</h1>
  <input @input="inputHandler" />
</template>
```

vue 用` v-bind:value=""`控制屬性<br>
不過要注意 `{{}}` 不可以放在 `""` 裡面<br>
但若是想把 msg 放進去 value 裡面，直接放入！<br>
縮寫為:`：value=""`(v-bind 為純冒號)<br>

```html
<script setup>
  import { ref } from "vue";

  const msg = ref("Hello World!");

  const inputHandler = (event) => {
    msg.value = event.target.value;
  };
</script>

<template>
  <h1>{{ msg }}</h1>
  <input v-bind:value="msg" @input="inputHandler" />
</template>
```

<h4>注意：只有v-on(@)和v-bind(:)有縮寫其他沒有</h4>
小題目 1:
若想要讓超過 15 字時，讓數字變成紅色。
關心的只是 msg 這個值，前面是 class 名字，後面是條件。

```html
<script setup>
  import { ref } from "vue";

  const msg = ref("Hello World!");

  const inputHandler = (event) => {
    msg.value = event.target.value;
  };
</script>

<template>
  <h1 :class="{ red: msg.length > 15 }">{{ msg }}</h1>
  <input v-model="msg" @input="inputHandler" />
</template>

<style>
  .red {
    color: red;
  }
</style>
```

小題目 2：當我點擊按鈕時，數字會做+1 的動作

```html
<script setup>
  import { ref } from "vue";

  const count = ref(0);
  const plus = () => count.value++;
</script>

<template>
  <h1>{{ count }}</h1>
  <button @click="plus">click me</button>
</template>
```

## 迴圈

`v-for="命名(item) in 前面的陣列或物件"`<br>
若是內容要放陣列或物件裡的東西，用兩個大括號包住：<br>
{{item.title}}<br>
{{item.skill}}<br>
<a href="https://play.vuejs.org/#eNq1lNtqE0EYx19lmJve1N3YVmhiGlDphV6oeLpxvIibSTJ1d2aYmY2BsCC0VVoNBgRTq1IrESO24AlPtPgy3d3kLZw95ACm3pTszRy+/zf7+w4zDXiOc6PmYpiDeWkJwhWQWLm8gChxOBMKNIDAZeCBsmAOmNHSGUQRtRiVCnDBSq6lJFgCtxEF+mskAwAIKqJsjGBOT/0n7WC/1X/bDT/tht/W+p1W0FlHcHYk5oJYsfj0XDYztk+cYiU9pKoUlznT5MSSrmPwKlNMmguZjDmfySCY+Hip72SOcG03/LHdazfDjRe9B1+DDzuTIeYWpggRbLZv3upvfQle7vmd5/679mSGxcXpIfidbX9zp7d6GO4/7HW3gld7xxQjm50aw9HPZv/pYf/xwdGf18Hvj+Gz75MZzpysFIjeOYto3kyaW7e1XijscLuosF7lS6QGaqfKTCzpw7UBEDpsawS1AoBIU/BbzbD7OQcaDRDJjDgS4Hl5M7KOZKu/gjcHI1kcxr+ycP19uPEI5IlT0X+/S2gpJ4WVIhhpkIWB02CMZmPscBYqqS9imVSMFcmovsNxqhG0mMOJjcUVroi+qFG+0iIgWLRtdv9SvKeEi9Pcap8qtu5N2F+R9STfVwWWWNQ02NCmiqKCVWJevn4Z1/V8aHR0EgfFPsZ4DUtmuxFjIjvv0pLGHtPFtBfjl4jQyg25XFeYykFQEWikTDoNQf06XfhP6CPceWMh9kPUg95fqfmz/A==">點擊看網址</a>

```html
<script setup>
  import { ref } from "vue";

  const products = [
    {
      title: "北歐風簡約餐椅",
      price: 1290,
      image: "https://picsum.photos/400/300",
    },
    {
      title: "無線藍牙耳機",
      price: 2490,
      image: "https://picsum.photos/400/300",
    },
    {
      title: "抗UV防曬外套",
      price: 880,
      image: "https://picsum.photos/400/300",
    },
    {
      title: "多功能筆記本",
      price: 199,
      image: "https://picsum.photos/400/300",
    },
    {
      title: "不鏽鋼保溫瓶",
      price: 590,
      image: "https://picsum.photos/400/300",
    },
  ];
</script>

<template>
  <div v-for="item in products">
    <div>名稱: {{ item.title }}</div>
    <div>價格: {{ item.price }}</div>
    <div>照片 <img v-bind:src="item.image" /></div>
  </div>
</template>
```

## Vue 的指令

開啟編輯器 Vscode 後在終端機執行<br>
執行流程如下：<br>

<p style="color:blue"><strong>一、初始化專案（建立專案結構）</strong></p>

```
npm init 版本+目錄名稱
```

▲ 為初始化一個新的 Node.js 專案<br>
使用後會建立一個名為 package.json 的檔案，這是 Node.js 專案的核心設定檔。<br>
如果加上 -y，則會跳過所有問題，使用預設值快速生成 package.json。<br>
<br>

<p style="color:blue"><strong>二、進入專案目錄</strong></p>

```
cd 進去目錄名稱
```

▲ 進入專案的目錄。<br>
使用終端機命令切換到剛剛用 npm init 初始化的專案目錄，準備在該目錄進行後續操作。

<p style="color:blue"><strong>三、安裝專案需要的所有套件</strong></p>

```
npm install
```

▲ 把專案依賴的套件裝起來，又叫 npm i<br>
當你的專案目錄下有一個 package.json 文件時，執行這個指令會自動安裝文件中列出的所有依賴。

<p style="color:blue"><strong>四、啟動前端開發伺服器</strong></p>

```
npm run dev
```

▲ 啟動一個 dev server 開發伺服器，會有網址就是成功<br>
npm run dev 是一個自定義腳本（script），在 package.json 中定義。<br>
通常在 Vue 專案中，這個指令會啟動 Vite 或 Webpack 的開發伺服器。<br>

<p style="color:blue"><strong>五、啟動 API 伺服器（若專案中需要）</strong></p>

```
npm run api（）

```

▲ 啟動專案中的 API 伺服器。<br>
與 npm run dev 一樣，這是定義在 package.json 的腳本指令。<br>
通常在開發環境中會啟動一個本地的 API 伺服器，提供前端專案調用的測試資料。<br>

<h3>裡面有Main.js ，它的啟動方式：</h3>
createApp 是 vue 的初始化<br>

```
import { createApp } from "vue";
import App from "./src/App.vue";

createApp(App).mount("#app");
```
