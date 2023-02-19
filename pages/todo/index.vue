<!-- pages/todo/index.vue -->
<template>
  <v-app>
    <v-content mt-0 pt-0>
      <v-card-title class="font-weight-bold">Todoリスト</v-card-title>
      <v-container>
        <v-row>
          <v-col>
            <v-text-field
              v-model="content"
              name="addName"
              label="タスクを入力してください"
              solo
            ></v-text-field>
          </v-col>
          <v-col>
            <v-btn outlline class="normal" @click="insert">追加</v-btn>
          </v-col>
        </v-row>
      </v-container>
      <v-container>
        <v-btn-toggle v-model="selected">
          <v-btn
            outlline
            class="green"
            :v-bind:class="{ 'is-active': !find_flg }"
            @click="flag_reset"
            >全て</v-btn
          >
          <v-btn
            outlline
            class="green"
            :v-bind:class="{ 'is-active': find_flg && find_state == '作業前' }"
            @click="find('作業前')"
            >作業前</v-btn
          >
          <v-btn
            outlline
            class="green"
            :v-bind:class="{ 'is-active': find_flg && find_state == '作業中' }"
            @click="find('作業中')"
            >作業中</v-btn
          >
          <v-btn
            outlline
            class="green"
            :v-bind:class="{ 'is-active': find_flg && find_state == '完了' }"
            @click="find('完了')"
            >完了</v-btn
          >
        </v-btn-toggle>
      </v-container>
      <v-container>
        <v-simple-table fixed-header height="500px">
          <template #default>
            <thead>
              <tr>
                <th>タスク</th>
                <th>登録日時</th>
                <th>状態</th>
                <th></th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, index) in display_todos" :key="index">
                <td>{{ item.content }}</td>
                <td>{{ item.created }}</td>
                <td>
                  <v-btn
                    outlline
                    rounded
                    class="gray"
                    :v-bind:class="{
                      'button--yet': item.state == '作業前',
                      'button--progress': item.state == '作業中',
                      'button--done': item.state == '完了',
                    }"
                    @click="changeState(item)"
                    >{{ item.state }}</v-btn
                  >
                </td>
                <td>
                  <v-btn outlline rounded class="blue" @click="remove(item)"
                    >削除</v-btn
                  >
                </td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import { mapState } from 'vuex'

export default {
  data: function () {
    return {
      content: '',
      find_state: '',
      find_flg: false,
      selected: undefined,
    }
  },
  computed: {
    ...mapState(['todos']),
    display_todos: function () {
      if (this.find_flg) {
        const arr = []
        const data = this.todos
        data.forEach((element) => {
          if (element.state === this.find_state) {
            arr.push(element)
          }
        })
        return arr
      } else {
        return this.todos
      }
    },
  },
  methods: {
    insert: function () {
      if (this.content !== '') {
        this.$store.commit('insert', { content: this.content })
        this.content = ''
      }
    },
    remove: function (todo) {
      this.$store.commit('remove', todo)
    },
    changeState: function (todo) {
      this.$store.commit('changeState', todo)
    },
    find: function (findState) {
      this.find_state = findState
      this.find_flg = true
    },
    flag_reset: function () {
      this.find_flg = false
    },
  },
}
</script>
