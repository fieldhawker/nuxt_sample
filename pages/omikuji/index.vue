<!-- pages/omikuji/index.vue -->
<template>
  <v-app>
    <v-main mt-0 pt-0>
      <v-card-title class="font-weight-bold">おみくじ</v-card-title>
      <v-container>
        <v-alert dense type="info">
          おみくじです<br />↓の「クリック」と書いてある所をクリック
        </v-alert>
      </v-container>
      <v-container>
        <div id="btn" @click="omikujiChange">{{ fortune }}</div>
      </v-container>
      <v-container>
        <v-card
          class="d-flex align-center justify-center pa-4 mx-auto"
          max-width="600"
          min-height="76"
        >
          <span class="text-h5">{{ message }}</span>
        </v-card>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>


export default {
  data: function () {
    return {
      fortune: 'クリック',
      omikujiArray: ['大吉', '吉', '中吉', '小吉', '末吉', '凶', '大凶'],
      omikujiMessage: [
        '極めつけのいいことが起こりそう',
        'ふつうよりはちょっといい',
        '今年、１回はいいことがありそう',
        'モテ期近し',
        'そんなに悪くない',
        '勢いだけはある',
        '羞恥心を捨てるとイイコトとありそう',
      ],
      message: '',
    }
  },
  methods: {
    omikujiChange: function () {
      const num = Math.floor(Math.random() * this.omikujiArray.length)
      this.fortune = this.omikujiArray[num]
      this.message = this.omikujiMessage[num]
    },
  },
}
</script>

<style>
#btn {
  width: 200px;
  height: 200px;
  background: #ef454a;
  border-radius: 50%;
  margin: 30px auto;
  text-align: center;
  line-height: 200px;
  color: #fff;
  font-weight: bold;
  font-size: 42px;
  cursor: pointer;
  box-shadow: 0 10px 0 #d1483e;
  user-select: none;
}
#btn:hover {
  opacity: 0.9;
}
#btn:active {
  box-shadow: 0 5px 0 #d1483e;
  margin-top: 35px;
}
</style>