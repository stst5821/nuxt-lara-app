<template>
  <div>
    <!-- 名前・本文投稿フォーム -->
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

    <!-- バリデーション結果 -->
    <p class="text-red-500 font-bold">{{ Validation.name_result }}</p>
    <p class="text-red-500 font-bold">{{ Validation.body_result }}</p>

    <!-- 投稿一覧 -->
    <hr class="my-4" />
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

export default {
  async asyncData({ $axios }) {
    const res = await $axios.$get(process.env.baseUrl + `api/index/`)
    return {
      results: res,
    }
  },
  data() {
    return {
      results: '',
      name: '',
      body: '',
      Validation: {
        name_result: '',
        body_result: '',
      },
    }
  },
  methods: {
    // laravelのPostControllerのstoreにデータを送る。
    submit() {
      let nameBool = false
      let bodyBool = false

      console.log(this.name)
      if (!this.name) {
        this.Validation.name_result = '名前を入力してください'
      } else {
        nameBool = true
        this.Validation.name_result = ''
      }

      if (!this.body) {
        this.Validation.body_result = '本文を入力してください'
      } else {
        bodyBool = true
        this.Validation.body_result = ''
      }

      if (nameBool === true && bodyBool === true) {
        // DBへの登録が完了したら、getでDBからデータを取ってきて追加したデータを画面に表示させる。
        axios
          .post(process.env.baseUrl + `api/store`, {
            name: this.name,
            body: this.body,
          })
          .then(() => {
            axios.get(process.env.baseUrl + `api/index`).then((response) => {
              this.results = response.data.result
            })
          })
        this.Validation.result = '送信に成功しました！'
        console.log(this.name)
        this.name = ''
        this.body = ''
        this.Validation.name_result = ''
        this.Validation.body_result = ''
      }
    },
    // メールフォームを追加予定のため残しておく
    checkString(inputdata) {
      const regex = /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/
      return regex.test(inputdata)
    },
  },
  remove(id) {
    axios.post(process.env.baseUrl + `api/delete`, { id }).then(() => {
      axios.get(process.env.baseUrl + `api/test1`).then((response) => {
        this.results = response.data.result
      })
    })
  },
}
</script>
