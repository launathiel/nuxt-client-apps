<template>
<div id="app">
  <h1>Posts Application</h1>

  <section v-if="errored">
    <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
  </section>

  <section v-else>
    <div v-if="loading">Loading...</div>

    <div v-else v-for="post in posts" class="post">
      {{ post.title }} with id = {{ post._id }}

      <div v-for="comment in comments">
        comments:
        {{comment.content}}
      </div>
      <br>
    </div>
  </section>
</div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      posts: null,
      comments:null
    }
  },
  async mounted () {
    await axios
      .get(process.env.postBaseUrl+'/posts/')
      .then(response => (this.posts = response.data.data))

    await axios
      .get(process.env.commentBaseUrl+'/posts/'+process.env.postId+'/comments')
      .then(res=>(this.comments = res.data.data))
  }
};
</script>

<style>
  body{
    background-color:#fefaeb;
    font-family: sans-serif;
    margin: 3%
  }
</style>