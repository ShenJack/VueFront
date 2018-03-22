<template>
  <div class="container">
    <article class="media" style="margin-left: 50px;margin-right: 50px">
      <figure class="media-left">
        <p class="image is-128x128">
          <img src="http://img.zcool.cn/community/015124597bf67ba8012193a32d741b.jpg@2o.jpg">
        </p>
      </figure>
      <div class="media-content">
        <div class="content">
          <p>
            <small></small>
            <strong>{{name}}</strong>
            <br>
          </p>
          <p>
            <small>创建于</small>
            <strong>{{createTime}}</strong>
          </p>
        </div>


        <nav class="level is-mobile">
          <div class="level-left">
            <a class="level-item">
              <span class="icon is-small"><i class="fas fa-reply"></i></span>
            </a>
            <a class="level-item">
              <span class="icon is-small"><i class="fas fa-retweet"></i></span>
            </a>
            <a class="level-item">
              <span class="icon is-small"><i class="fas fa-heart"></i></span>
            </a>
          </div>
        </nav>
      </div>
      <div class="media-right">
      </div>
    </article>
    <div class="level" style="margin-top: 40px">
      <div class="level-item has-text-centered">
        <div>
          <p class="heading">账号</p>
          <p class="title">{{username}}</p>
        </div>
      </div>
      <div class="level-item has-text-centered">
        <div>
          <p class="heading">密码</p>
          <p class="title">{{password}}</p>
        </div>
      </div>
      <div class="level-item has-text-centered">
        <div>
          <p class="heading">年级</p>
          <p class="title">{{book}}</p>
        </div>
      </div>
      <div class="level-item has-text-centered">
        <div>
          <p class="heading">更新的时间</p>
          <p class="title">{{updateTime}}</p>
        </div>
      </div>
    </div>
    <div class="container" style="text-align: end; margin-right: 50px">信息不对？试试<a @click="editProfile()">编辑</a></div>
  </div>
</template>

<script>
  export default {
    name: 'bigtitle',
    data() {
      return {
        name: '',
        answerStatus: '',
        username: '',
        password: '',
        book: '',
        createTime: '',
        updateTime: '',
        answerPercent: '',
      }
    },
    methods:{
      editProfile:function () {
        this.$emit('editProfile',true);
      },
      setProfile:function (data) {
        this.name = data.name;
        this.username = data.username;
        this.password = data.password;
        this.createTime = data.createTime;
        this.updateTime = data.updateTime;

        this.book = data.book;
      }
    }
  }

</script>
