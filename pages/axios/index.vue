<!-- pages/axios/index.vue -->
<template>
  <div class="page">
    <ul>
      <li v-for="post in posts" :key="post.id">
        {{ post.title }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      posts: [],
    }
  },
  mounted() {
    let result

    axios
      .get('https://jsonplaceholder.typicode.com/posts')
      .then(function (response) {
        result = response
      })

    if (result != null) {
      this.posts = result.data
    }
  },
}
</script>