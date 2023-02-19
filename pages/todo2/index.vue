<!-- pages/todo2/index.vue -->
<template>
  <v-app>
    <v-main mt-0 pt-0>
      <v-card-title class="font-weight-bold"
        >Todoリスト(LocalStorage対応)リロードしても残る系</v-card-title
      >
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
    </v-main>
  </v-app>
</template>

<script>
export default {
  data: function () {
    return {
      content: '',
      find_state: '',
      find_flg: false,
      selected: undefined,
      todos: [],
      option: [
        { id: 0, label: '作業前' },
        { id: 1, label: '作業中' },
        { id: 2, label: '完了' },
      ],
    }
  },
  computed: {
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
  mounted: function () {
    if (localStorage.getItem('todos')) {
      try {
        this.todos = JSON.parse(localStorage.getItem('todos'))
      } catch (e) {
        localStorage.removeItem('todos')
      }
    }
  },
  methods: {
    find: function (findState) {
      this.find_state = findState
      this.find_flg = true
    },
    flag_reset: function () {
      this.find_flg = false
    },

    insert: function (obj) {
      if (this.content !== '') {
        const d = new Date()
        const fmt =
          d.getFullYear() +
          '-' +
          ('00' + (d.getMonth() + 1)).slice(-2) +
          '-' +
          ('00' + d.getDate()).slice(-2) +
          ' ' +
          ('00' + d.getHours()).slice(-2) +
          ':' +
          ('00' + d.getMinutes()).slice(-2)
        this.todos.unshift({
          content: this.content,
          created: fmt,
          state: '作業前',
        })

        this.content = ''
        this.saveWords()
      }
    },
    remove: function (obj) {
      for (let i = 0; i < this.todos.length; i++) {
        const ob = this.todos[i]
        if (ob.content === obj.content && ob.created === obj.created) {
          if (confirm('"' + ob.content + '"を削除しますか？')) {
            this.todos.splice(i, 1)
            this.saveWords()
            return
          }
        }
      }
    },
    changeState: function (obj) {
      for (let i = 0; i < this.todos.length; i++) {
        const ob = this.todos[i]
        if (
          ob.content === obj.content &&
          ob.created === obj.created &&
          ob.state === obj.state
        ) {
          let nowState
          for (let j = 0; j < this.option.length; j++) {
            if (this.option[j].label === ob.state) {
              nowState = this.option[j].id
            }
          }
          nowState++
          if (nowState >= this.option.length) {
            nowState = 0
          }
          obj.state = this.option[nowState].label
          this.saveWords()
          return
        }
      }
    },
    saveWords() {
      const parsed = JSON.stringify(this.todos)
      localStorage.setItem('todos', parsed)
    },
  },
}
</script>
