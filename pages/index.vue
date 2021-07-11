<template>
  <div>
    <form @submit.prevent>
      <label
        >名前：
        <input
          v-model="name"
          placeholder="入力ください"
          type="text"
          name="entry"
          @keyup.enter="submit"
        />
      </label>
      <br />
      <label
        >本文：
        <input
          v-model="body"
          placeholder="入力ください"
          type="text"
          name="entry"
          @keyup.enter="submit"
        />
      </label>
      <button
        class="
          bg-blue-500
          hover:bg-blue-700
          text-white
          font-bold
          py-1
          px-2
          rounded
        "
        type="submit"
        @click="submit"
      >
        送信
      </button>
    </form>
    <!-- <div>
      <p>{{ $store.state.results }}</p>
    </div> -->
    <ul>
      <li v-for="result in results" :key="result.id">
        id:
        <nuxt-link :to="'/posts/' + result.id">{{ result.id }}</nuxt-link>
        | name:{{ result.name }} | 本文:{{ result.body }}
        <button @click="remove(result.id)">X</button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'
// import { mapState } from 'vuex'

export default {
  data() {
    return {
      results: '',
      name: '',
      body: '',
    }
  },
  mounted() {
    if (process.env.production === 'production') {
      axios.get(apiBaseUrl + '/test1')
    }
    axios
      .get('http://localhost/api/test1')
      .then((response) => {
        this.results = response.data.result
        console.log(this.results)
      })
      .catch((error) => {
        console.log(error)
        this.result = 'ERROR'
      })
  },
  methods: {
    // laravelのPostControllerのstoreにデータを送る。
    submit() {
      const CORS_PROXY = 'http://localhost/'
      // DBへの登録が完了したら、getでDBからデータを取ってきて追加したデータを画面に表示させる。
      axios
        .post(CORS_PROXY + 'api/store', { name: this.name, body: this.body })
        .then(() => {
          axios.get(CORS_PROXY + 'api/test1').then((response) => {
            this.results = response.data.result
            console.log(this.results)
          })
        })
    },
    remove(id) {
      const CORS_PROXY = 'http://localhost/'
      axios.post(CORS_PROXY + 'api/delete', { id }).then(() => {
        axios.get(CORS_PROXY + 'api/test1').then((response) => {
          this.results = response.data.result
          console.log(this.results)
        })
      })
    },
  },
}
</script>
