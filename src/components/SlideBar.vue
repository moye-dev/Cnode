<template>
<div class="autherinfo">
    <div class="authersummray">
        <div class="topbar">作者</div>
        <div style="background-color:white;">
            <router-link :to="{
                    name: 'user_info',
                    params:  {
                      name: userinfo.loginname
                    }
                    }">
                    <img :src="userinfo.avatar_url" alt="">
            </router-link>
            <router-link :to="{
                    name: 'user_info',
                    params:  {
                      name: userinfo.loginname
                    }
                    }">
                    <span style="color:#8c828c;">{{userinfo.loginname}}</span>
            </router-link>
        <p style="color:#525c64;margin-left:10px;">积分：{{userinfo.score}}</p>
        </div>
    </div>
    <div class="recent_topics">
        <div class="topbar" style="color:#525c64;font-size:13px;">作者其他话题</div>
        <ul>
            <li v-for="(list,index) in topiclimitby5" :key="index">
                <router-link :to="{
                    name: 'post_content',
                    params:{
                        id: list.id,
                        name: list.author.loginname
                    }
                }">
                    {{list.title}}
                </router-link>
            </li>
        </ul>
    </div>
    <div class="recent_replies">
        <div class="topbar" style="color:#525c64;font-size:13px;">无人回复的话题</div>
        <ul>
            <li v-for="(list,index) in replylimitby5" :key="index">
                <router-link :to="{
                    name: 'post_content',
                    params:{
                        id: list.id,
                        name: list.author.loginname
                    }
                }">
                    {{list.title}}
                </router-link>
            </li>
        </ul>
    </div>
</div>
</template>
<script>
export default {
  name: 'SlideBar',
  data () {
    return {
      isLoading: false,
      userinfo: {}
    }
  },
  methods: {
    getData () {
      this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res => {
          this.isLoading = false
          this.userinfo = res.data.data
        })
        .catch(function (err) {
          console.log(err)
        })
    }
  },
  computed: {
    topiclimitby5 () {
      if (this.userinfo.recent_topics) {
        return this.userinfo.recent_topics.slice(0, 5)
      }
    },
    replylimitby5 () {
      if (this.userinfo.recent_replies) {
        return this.userinfo.recent_replies.slice(0, 5)
      }
    }
  },
  beforeMount () {
    this.isLoading = true
    this.getData()
  }
}
</script>
<style scoped>
  .authersummay, .recent_replies, .recent_topics {
    background-color: #fff;
  }
  .autherinfo {
    width: 328px;
    float: right;
    margin-top: 0;
  }
  li{
    padding: 3px 0 ;
  }
  .recent_replies ul, .recent_topics ul {
    margin-top: 0px;
    margin-bottom: 0px;
    list-style: none;
    padding-left: 14px;
  }

  ul a {
    font-size: 12px;
    text-decoration: none;
    color: #778087;
  }

  .topbar {
    padding: 10px;
    background-color: #f6f6f6;
    height: 16px;
    font-size: 12px;
    margin-top: 10px;
  }

  img {
    height: 48px;
    width: 48px;
    border-radius: 3px;
    margin: 10px;
  }

  .loginname {
    width: 100px;
    float: right;
    margin-top: 22px;
    margin-right: 159px;
    font-size: 14px;
  }

  .loginname a {
    text-decoration: none;
    color: #778087;
  }

  .authersummay .topbar {
    margin-top: 0px;
  }
</style>
