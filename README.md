# nuxt-lara-app

## Build Setup

vercel へのデプロイ

setting の build command で、override のトグルをオンにして
$ nuxt generate
と入力。

そのほかはそのままで OK。

pages.index.vue の記述を以下のようにする。

```
<script>
export default {
  async asyncData({ app }) {
    const data = await app.$axios.$get(
      `https://laraveltestapp111.herokuapp.com/api` // ←herokuに上げたLaravelのURL + /api と書く。
    )
    return { data }
  },
}
</script>
```

ここの URL を間違えると、networkerror というエラー画面になる。
