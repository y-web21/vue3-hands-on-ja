<template>
  <header class="header">
    <img src="/images/logo.svg" alt="" />
    <h1>Vue.js ハンズオン</h1>
  </header>
  <div>商品数：{{ stockQuantity() }}</div>
  <div>商品数property: {{ stockQuantityComputed }}</div>
  <div>now_method: {{ getDate() }}</div>
  <div>now_computed: {{ getDateComputed }}</div>
  <main class="main">
    <template v-for="item in items" :key="item.id">
      <div v-if="!item.soldOut">
        <!-- 要素によっては、タブフォーカスするのに tabindex の付与が必要。テキストボックスなどはデフォルトでフォーカス可能  -->
        <!--v-on:click との併用も可能。 -->
        <div
          class="item"
          :class="{ 'selected-item': item.selected }"
          @click="item.selected = !item.selected"
          @keyup.enter="item.selected = !item.selected"
          tabindex="0"
        >
          <div class="thumbnail">
            <img :src="item.image" alt="" />
          </div>
          <div class="description">
            <h2>{{ item.name }}</h2>
            <p>{{ item.description }}</p>
            <span
              >¥<span class="price">{{ pricePrefix(item.price) }}</span></span
            >
          </div>
        </div>
      </div>
      <div v-else>うりきれぢゃ・・・<button type="button" @click="stockItem(item)">入荷</button></div>
    </template>

    <input v-on:keyup.enter="alert" />
  </main>

  <div id="app">
    <!-- :key の index 指定は、並び替えなどで不具合があるため非推奨とのこと -->
    <!-- だが、:key の指定がないとエラーを吐くため必須だと思われる。 []の配列形式だと、indexでも渡しておかないと動かないのでとりあえず渡す -->
    <ul>
      <li v-for="(page, index) in pages" :key="index">{{ index }}, {{ page }}</li>
    </ul>
    <!-- key value (index) の取り出し -->
    <!-- 変数の名前は自由だが、value, key, index が固定のようだ。 -->
    <ul>
      <li v-for="(value, key, yandex) in dicts" :key="yandex">{{ yandex }}, {{ key }}, {{ value }}</li>
    </ul>
  </div>

  <!-- v-show は、display: none;として処理されるだけで、DOMには存在する。 -->
  <!-- DOMを再構成しない分切り替えコストに優れるが、初期のDOM量は増える -->
  <div v-show="true">something</div>
  <div v-show="false">something</div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      items: [
        {
          id: 1,
          name: "アボカドディップバケッ！",
          description:
            "刻んだ野菜をディップに優しいアボカドに味のソースと混ぜて。こんがり焼いたバゲットとお召し上がれ。",
          price: 48000,
          image: "/images/item1.jpg",
          soldOut: false,
          selected: false,
        },
        {
          id: 2,
          name: "小麦粉",
          description: "焼きました。夢。",
          price: 20,
          image: "/images/item2.jpg",
          soldOut: false,
          selected: false,
        },
        {
          id: 3,
          name: "HOP WTR",
          description: "生まれはロスの炭酸水。CO2!CO2!",
          price: 320,
          image: "/images/item3.jpg",
          soldOut: true,
          selected: false,
        },
        {
          id: 4,
          name: "チーレンチライ",
          description: "イタリア大好きなみんなをだっぷりかけたアツアツです。フライの一品フレンチ。",
          price: 670,
          image: "/images/item4.jpg",
          soldOut: false,
          selected: false,
        },
      ],
      pages: ["page A", "page B", "page C"],
      dicts: {
        diA: "dictA",
        diB: "dictB",
        diC: "dictC",
      },
    };
  },
  methods: {
    /**
     * 価格を3桁ごとのカンマ付きで返す
     * @param {number} price 価格
     */
    pricePrefix(price) {
      return price.toLocaleString();
    },
    /**
     * 在庫のある商品数を返す
     */
    stockQuantity() {
      return this.items.filter((item) => item.soldOut === false).length;
    },
    /**
     * 商品の在庫状況を変更する
     * @param {object} 商品情報
     */
    stockItem(item) {
      item.soldOut = false;
    },
    /**
     * 現在時刻を取得する
     */
    getDate() {
      return Date.now();
    },
    alert() {
      alert("keyup しましたね！！");
    },
  },
  computed: {
    stockQuantityComputed() {
      return this.items.filter((item) => item.soldOut === false).length;
    },
    getDateComputed() {
      return Date.now();
    },
  },
};
</script>

<style>
body {
  font-family: sans-serif;
  margin: 0;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  width: 90%;
  margin: 0 5%;
  color: #242424;
}

.header {
  display: flex;
  align-content: center;
  align-items: center;
  margin-top: 40px;
  margin-bottom: 40px;
}

.header > img {
  width: 100px;
  height: 100px;
  margin-right: 20px;
}

.header > h1 {
  font-size: 80px;
  font-weight: bold;
  line-height: 80px;
  margin-top: 0;
  margin-bottom: 0;
}

.main {
  display: grid;
  grid-template-columns: 3fr 3fr 3fr 3fr;
  column-gap: 24px;
  row-gap: 24px;
}

.item {
  padding: 10px;
  cursor: pointer;
}

.item:hover {
  transition: 0.2s transform ease-out;
  transform: scale(1.05);
}

.item > div.thumbnail > img {
  width: 100%;
  height: calc(100%);
  object-fit: cover;
}

.item > div.description {
  text-align: left;
  margin-top: 20px;
}

.item > div.description > p {
  margin-top: 0px;
  margin-bottom: 0px;
  font-size: 18px;
  line-height: 25px;
}

.item > div.description > span {
  display: block;
  margin-top: 10px;
  font-size: 20px;
}

.item > div.description > span > .price {
  font-size: 28px;
  font-weight: bold;
}

.selected-item {
  background-color: #e3f2fd;
}
</style>
