<template>
  <div>
    <p>編集画面</p>
    id:{{ results.id }}

    <form @submit.prevent>
      <input
        v-model="results.id"
        placeholder="入力ください"
        type="hidden"
        name="entry"
        @keyup.enter="submit"
      />
      <br />
      <label
        >名前：
        <input
          v-model="results.name"
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
          v-model="results.body"
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
        編集完了
      </button>
    </form>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  async asyncData(context) {
    const res = await context.$axios.$get(
      process.env.baseUrl + `api/edit/${context.params.id}`
    )

    return {
      results: res.result,
    }
  },
  data() {
    return {
      id: '',
      name: '',
      body: '',
    }
  },
  methods: {
    // laravelのPostControllerのstoreにデータを送る。
    submit() {
      // DBへの登録が完了したら、getでDBからデータを取ってきて追加したデータを画面に表示させる。
      axios
        .post(process.env.baseUrl + 'api/update', {
          id: this.results.id,
          name: this.results.name,
          body: this.results.body,
        })
        .then(() => {
          axios.get(process.env.baseUrl + `api/edit/68`).then((response) => {
            this.result = response.data.result
          })
        })
    },
  },
}
</script>
