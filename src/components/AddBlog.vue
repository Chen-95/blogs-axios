<template>
  <div id="add-blog">
    <h2>添加博客</h2>
    <form v-if="!submmited">
      <label>博客标题</label>
      <input type="text" name id v-model="blog.title" required>
      <label>博客内容</label>
      <textarea v-model="blog.content"></textarea>
      <div id="checkboxes">
        <label>Vue.js</label>
        <input type="checkbox" value="Vue.js" name id v-model="blog.categories">
        <label>Node.js</label>
        <input type="checkbox" value="Node.js" name id v-model="blog.categories">
        <label>React.js</label>
        <input type="checkbox" value="React.js" name id v-model="blog.categories">
        <label>Angular4</label>
        <input type="checkbox" value="Angular4" name id v-model="blog.categories">
      </div>
      <label>作者：</label>
      <select v-model="blog.author">
        <option v-for="(author,index) in authors" :key="index">{{author}}</option>
      </select>
      <button @click.prevent="post">添加博客</button>
    </form>
    <div v-if="submmited">
      <h3>您的博客添加成功！</h3>
    </div>
    <hr>
    <div id="preview">
      <h3>博客总览</h3>
      <p>博客标题：{{blog.title}}</p>
      <p>博客内容：</p>
      <p>{{blog.content}}</p>
      <p>博客类型：</p>
      <ul>
        <li v-for="(category,index) in blog.categories" :key="index">{{category}}</li>
      </ul>
      <p>作者：{{blog.author}}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: "add-blog",
  data() {
    return {
      blog: {
        title: "",
        content: "",
        categories: [],
        author: ""
      },
      authors: ["匿名", "Nice", "Mr.Chen", "ZhangLi"],
      submmited: false
    };
  },
  methods: {
    post: function() {
        //var _this = this;
      //this.$http.post("https://wd0314437253xaoamp.wilddogio.com/posts.json", this.blog)
        axios.post("https://wd0314437253xaoamp.wilddogio.com/posts.json", this.blog)
        .then((data) => {
          //console.log(data);
          //_this.submmited = true;
          this.submmited = true;
        });
    }
  }
};
</script>

<style scoped>
#add-blog * {
  box-sizing: border-box;
}
#add-blog {
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
