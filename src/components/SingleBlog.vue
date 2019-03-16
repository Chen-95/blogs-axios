<template>
  <div id="single-blog">
    <h1>{{blog.title}}</h1>
    <article>{{blog.content}}</article>
    <p>作者：{{blog.author}}</p>
    <p>分类：</p>
    <ul>
      <li v-for="(categorie,index) in blog.categories" :key="index">{{categorie}}</li>
    </ul>
    <hr>
    <router-link :to="'/edit/' + id">
      <button id="edit">编辑</button>
    </router-link>

    <button @click="deleteSingleBlog()" id="delete">删除</button>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "single-blog",
  data() {
    return {
      id: this.$route.params.id,
      blog: {}
    };
  },
  created() {
    axios.get(
        "https://wd0314437253xaoamp.wilddogio.com/posts/" + this.id + ".json"
      )
      .then(result => {
        this.blog = result.data;
      })
      .catch(err => {
        console.log(err);
      });
  },
  methods: {
    deleteSingleBlog() {
      axios
        .delete(
          "https://wd0314437253xaoamp.wilddogio.com/posts/" + this.id + ".json"
        )
        .then(result => {
          this.$router.push({ path: "/" });
        })
        .catch(err => {
          console.log(err);
        });
    }
  }
};
</script>
<style scoped>
#single-blog {
  max-width: 960px;
  margin: 0 auto;
  padding: 20px 20px 42px 20px;
  background: #eee;
  border: 1px dotted #aaa;
}
#delete {
  padding: 8px;
  color: #fff;
  background: #FE7300;
  border: 0;
  border-radius: 5px;
  font-size: 15px;
  float: right;
  margin-right: 10px;
}
#edit {
  padding: 8px;
  color: #fff;
  background: #FE7300;
  border: 0;
  border-radius: 5px;
  font-size: 14px;
  float: right;
}
</style>

