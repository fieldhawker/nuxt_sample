<!-- pages/omikuji/index.vue -->
<template>
  <v-app>
    <v-main mt-0 pt-0>
      <v-card-title class="font-weight-bold">文字列のローカル保存</v-card-title>
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
              v-model="newWord"
              name="newWord"
              label="文字列を入力してください"
              solo
            ></v-text-field>
          </v-col>
          <v-col>
            <v-btn outlline class="normal" @click="addWord">追加</v-btn>
          </v-col>
        </v-row>

        <v-row>
          <v-card
            v-for="(word, n) in words"
            :key="word"
            width="150"
            class="ma-2 d-flex flex-column"
          >
            <v-card-title>{{ word }}</v-card-title>
            <v-card-text>
              <v-btn outlline rounded class="gray" @click="removeWord(n)"
                >削除</v-btn
              >
            </v-card-text>
          </v-card>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data: function () {
    return {
      words: [],
      newWord: null,
    }
  },
  mounted: function () {
    if (localStorage.getItem('words')) {
      try {
        this.words = JSON.parse(localStorage.getItem('words'))
      } catch (e) {
        localStorage.removeItem('words')
      }
    }
  },
  methods: {
    addWord() {
      if (!this.newWord) {
        return
      }

      this.words.push(this.newWord)
      this.newWord = ''
      this.saveWords()
    },
    removeWord(x) {
      this.words.splice(x, 1)
      this.saveWords()
    },
    saveWords() {
      const parsed = JSON.stringify(this.words)
      localStorage.setItem('words', parsed)
    },
  },
}
</script>

<style>
</style>
