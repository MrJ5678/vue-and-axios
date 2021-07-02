<!--
 * @Author: hhhhhq
 * @Date: 2021-07-02 09:22:36
 * @LastEditors: hhhhhq
 * @LastEditTime: 2021-07-02 09:57:05
 * @Description: file content
-->
<template>
  <div v-if="isLoading">Loading ...</div>
  <div v-else-if="errorMsg">
    {{ errorMsg }}
  </div>
  <div v-else>
    <i>"{{ quote }}"</i>
    <p>- Kanye West</p>
  </div>
  <p>
    <button @click="createPost">Create Post</button>
  </p>
</template>

<script>
import axios from "axios"
import { ref } from "vue"
import KanyeAPI from "./services/KanyeAPI"

export default {
  setup() {
    const quote = ref("")
    const errorMsg = ref(null)
    const isLoading = ref(false)

    // const loadQuote = async () => {
    //   let response = await axios.get("https://api.kanye.rest/")
    //   quote.value = response.data.quote
    // }
    const loadQuote = async () => {
      isLoading.value = true
      try {
        let response = await KanyeAPI.getQuote()
        quote.value = response.data.quote
        isLoading.value = false
      } catch (error) {
        // console.log(error.message)
        errorMsg.value = error.message
        isLoading.value = false
      }
    }

    loadQuote()

    // axios.get("https://api.kanye.rest/").then(response => {
    //   console.log(response)
    //   quote.value = response.data.quote
    // })

    const createPost = async () => {
      isLoading.value = true
      try {
        const response = await KanyeAPI.createPost(
          JSON.stringify({
            title: "foo",
            body: "bar",
            userId: 1,
          })
        )
        console.log(response)
        isLoading.value = false
      } catch (error) {
        console.dir(error)
        errorMsg.value = error.message
        isLoading.value = false
      }

      // axios
      //   .post(
      //     "https://jsonplaceholder.typicode.com/posts",
      //     JSON.stringify({
      //       title: "foo",
      //       body: "bar",
      //       userId: 1,
      //     })
      //   )
      //   .then(response => {
      //     console.log(response)
      //     isLoading.value = false
      //   })
    }

    return {
      quote,
      errorMsg,
      isLoading,
      createPost,
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
