<template>
  <div id="show-blogs" v-theme:column="'narrow'">
    <h1>博客总览</h1>
    <input type="text" placeholder="搜索" v-model="search">
    <div class="single-blog" v-for="(blog,index) in filteredBlogs" :key="index">
      <router-link :to="'/blog/'+blog.id">
        <h2 v-rainbow>{{blog.title | to-uppercase}}</h2>
        <article>{{blog.content | snippet}}</article>
      </router-link>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "show-blogs",
  data() {
    return {
      blogs: [],
      search: ""
    };
  },
  created() {
    axios
      .get("https://wd0314437253xaoamp.wilddogio.com/posts.json")
      .then(result => {
        //console.log(result.data);
        return result.data;
      })
      .then(result => {
        var blogsArray = [];
        for (let key in result) {
          result[key].id = key;
          blogsArray.push(result[key]);
        }
        //this.blogs = blogsArray.slice(0, 10);//显示10条
        this.blogs = blogsArray; //全部显示
      })
      .catch(err => {
        console.log(err);
      });
  },
  computed: {
    filteredBlogs: function() {
      return this.blogs.filter(blog => {
        return blog.title.match(this.search);
      });
    }
  },
  //全局自定义过滤器
  filters: {
    //格式一
    "to-uppercase": function(value) {
      return value.toUpperCase();
    },
    //格式二
    snippet(value) {
      return value.slice(0, 150) + "...";
    }
  },
  //全局自定义指令
  directives: {
    rainbow: {
      bind(el, binding, vnode) {
        el.style.color =
          "#" +
          Math.random()
            .toString(16)
            .slice(2, 8);
      }
    },
    theme: {
      bind(el, binding, vnode) {
        if (binding.value == "wide") {
          el.style.maxWidth = "1060px";
        } else if (binding.value == "narrow") {
          el.style.maxWidth = "660px";
        }

        if (binding.arg == "column") {
          el.style.background = "#6677CC";
          el.style.padding = "20px";
        }
      }
    }
  }
};
</script>
<style scoped>
#show-blogs {
  max-width: 800px;
  margin: 0 auto;
}
.single-blog {
  padding: 20px;
  margin: 20px 0;
  box-sizing: border-box;
  background: #eee;
  border: 1px dotted #aaa;
}
#show-blogs a {
  text-decoration: none;
}
input[type="text"] {
  padding: 6px;
  width: 100%;
  box-sizing: border-box;
}
</style>

