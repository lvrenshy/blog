<template>
  <div class="hello">
    <h1>{{ blogDetail.blog.title }}</h1>

    <div>
      <h1 class="postTitle">

        <b>用户名: {{ blogDetail.blog.author }}</b>

      </h1>
      <div v-html="blogDetail.blog.content"></div>
    </div>


    <div class="comment-container">
      <template v-for="({commentator,content,comment_time},index) in blogDetail.comments">

        <div :key="index">
          <div class="feedbackListSubtitle">
            <b style="margin-right: 15px">{{ comment_time }}</b>
            <b>{{ commentator }}</b>
          </div>
          <div class="feedbackCon">
            {{ content }}
          </div>
        </div>

      </template>

    </div>
    <p>

      <b>用户名:</b>
      <input type="text" v-model="commentator">
    </p>


    <p>
      <b> 评论:</b>

      <input type="text" v-model="content">
    </p>
    <button @click="commitComment">评论</button>
  </div>
</template>

<script>

import * as axios from "axios";

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      blogDetail: {
        blog: {},
        comments: {}
      },
      content: '',
      commentator: ""
    }
  },
  created() {
    //  发送请求
    this.send();
  },
  methods: {
    send() {
      axios({//格式a
        method: 'get',
        url: 'http://localhost:8000/blog/detail?id=1'
      }).then((response) => {
        console.log(response.data);
        this.blogDetail = response.data;
      }).catch(error => {
        console.log(error)
      });
    },
    commitComment() {
      let {content, commentator} = this

      axios({//格式a
        method: 'post',
        url: 'http://l' +
            'ocalhost:8000/blog/comment',
        data: {
          content,
          commentator
        },
        headers: {
          'Content-Type': "application/json"
        }
      }).then(() => {
        alert("评论成功")
      }).catch(() => {
        alert("评论失败")
      });
    }


  }

}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

#topics .postTitle {
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 20px;
  line-height: 1.8;
  color: #333;
}

.feedbackListSubtitle {
  width: 80%;
  height: 40px;
  margin: 0 auto;
  text-align: left;
  padding: 15px;
}


.feedbackCon {
  width: 80%;
  height: 150px;
  margin: 0 auto;
}
</style>
