<!-- pages/axios/index.vue -->
<template>
  <div class="page">
    <v-alert dense type="info">
      {{ year }}年{{ month }}月{{ day }}日時点の累計コロナ感染者数
    </v-alert>

    <v-row>
      <v-card
        v-for="item in itemList"
        :key="item.name_jp"
        width="200"
        class="ma-2 d-flex flex-column"
      >
        <v-card-title>{{ item.name_jp }}</v-card-title>
        <v-card-text>{{ item.npatients | numberFormat }}人</v-card-text>
      </v-card>
    </v-row>
  </div>
</template>

<script>
export default {
  filters: {
    numberFormat: (text) => {
      return Number(text).toLocaleString()
    },
  },
  async asyncData(context) {
    const hiduke = new Date()
    hiduke.setDate(hiduke.getDate() - 2) // 昨日分がAPIに反映されるまで空になるので一昨日
    const year = hiduke.getFullYear()
    const month = ('0' + (hiduke.getMonth() + 1)).slice(-2)
    const day = ('0' + hiduke.getDate()).slice(-2)
    const url = '/api/Covid19JapanAll?date=' + year + month + day // https://opendata.corona.go.jp/
    console.log(url)
    
    const corona = await context.$axios.$get(
      // "https://jsonplaceholder.typicode.com/posts",
      url,
      {
        params: {},
      }
    )
    console.log(corona)
    return { itemList: corona.itemList, year, month, day }
  },
  data() {
    return {
      posts: {
        post: {
          userId: 1,
          id: 1,
          title:
            'sunt aut facere repellat provident occaecati excepturi optio reprehenderit',
          body: 'quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto',
        },
      },
      itemList: [],
      year: '----',
      month: '--',
      day: '--',
    }
  },
}
</script>