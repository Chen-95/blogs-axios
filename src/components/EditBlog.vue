<template>
  <div id="edit-blog">
    <h2>编辑博客</h2>
    <form v-if="!submmited">
      <label>博客标题</label>
      <input type="text" v-model="blog.title" required>
      <label>博客内容</label>
      <textarea v-model="blog.content"></textarea>
      <div id="checkboxes">
        <label>Vue.js</label>
        <input type="checkbox" value="Vue.js" v-model="blog.categories">
        <label>Node.js</label>
        <input type="checkbox" value="Node.js" v-model="blog.categories">
        <label>React.js</label>
        <input type="checkbox" value="React.js" v-model="blog.categories">
        <label>Angular4</label>
        <input type="checkbox" value="Angular4" v-model="blog.categories">
      </div>
      <label>作者：</label>
      <select v-model="blog.author">
        <option v-for="(author,index) in authors" :key="index">{{author}}</option>
      </select>
      <button @click.prevent="put">编辑博客</button>
    </form>
    <div v-if="submmited">
      <h3>您的博客编辑成功！</h3>
    </div>
    <hr>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "edit-blog",
  data() {
    return {
      id: this.$route.params.id,
      blog: {},
      authors: ["匿名", "Nice", "Mr.Chen", "ZhangLi"],
      submmited: false
    };
  },
  methods: {
    fetchData() {
      axios.get(
          "https://wd0314437253xaoamp.wilddogio.com/posts/" + this.id + ".json"
        )
        .then(result => {
          this.blog = result.data;
          if (typeof result.data.categories === "object") {
            var arr = [];
            for (let i in result.data.categories) {
              arr.push(result.data.categories[i]);
            }
            this.blog.categories = arr;
          }
          if (!Array.isArray(result.data.categories)) {
            this.blog.categories = [];
          }
        })
        .catch(err => {
          console.log(err);
        });
    },
    put() {
      axios.put(
          "https://wd0314437253xaoamp.wilddogio.com/posts/" + this.id + ".json",
          this.blog
        )
        .then((data)=> {
          this.submmited = true;
        });
    }
  },
  created() {
    this.fetchData();
  }
};
</script>

<style scoped>
#edit-blog * {
  box-sizing: border-box;
}
#edit-blog {
  margin: 20px auto;
  max-width: 600px;
  padding: 20px;
}
label {
  display: block;
  margin: 20px 0 10px;
}
input[type="text"],
textarea,
select {
  display: block;
  width: 100%;
  padding: 8px;
}
textarea {
  height: 200px;
}
#checkboxes label {
  display: inline-block;
  margin-top: 10px;
}
#checkboxes input {
  display: inline-block;
  margin-right: 30px;
}
button {
  display: block;
  margin: 20px 0;
  background: crimson;
  color: #fff;
  border: 0;
  padding: 14px;
  border-radius: 5px;
  font-size: 18px;
  cursor: pointer;
}
#preview {
  padding: 10px 20px;
  border: 1px dotted #ccc;
  margin: 30px 0;
}
h3 {
  margin-top: 10px;
}
</style>
