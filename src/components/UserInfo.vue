<template>
   <div class="UserInfo">
      <div class="loading" v-if="isLoading">
            <img src="../assets/loading.gif">
      </div>
    <div class="userInfomation" v-else>
        <section>
            <img :src="userinfo.avatar_url">
            <span style="color:#918ea4;">{{userinfo.loginname}}</span>
            <p style="color:#1c96d2;">{{userinfo.score}}积分</p>
            <p style="color:#918ea4;">个收藏</p>
            <p style="color:#bcc4d2;">注册时间：{{userinfo.create_at | formatDate}}</p>
        </section>
        <div class="topics">
            <p>最近创建的主题</p>
            <ul>
                <li v-for="(item,index) in userinfo.recent_topics" :key="index">
                    <img :src="userinfo.avatar_url">
                    <router-link :to="{
                        name:'post_content',
                        params:{
                        id:item.id
                        }
                        }">
                        {{item.title}}
                    </router-link>
                </li>
            </ul>
        </div>
        <div class="replies">
            <p>最近参与的话题</p>
            <ul>
                <li v-for="(item,index) in userinfo.recent_replies" :key="index">
                    <router-link :to="{
                        name:'post_content',
                        params:{
                        id:item.id
                        }
                        }">
                        {{item.title}}
                    </router-link>
                </li>
            </ul>
        </div>
    </div>
   </div>
</template>
<script>
export default {
  name: 'UserInfo',
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
  beforeMount () {
    this.isLoading = true
    this.getData()
  }
}
</script>
<style scoped>
  .userInfomation {
    background: white;
    width: 75%;
    margin: 10px auto;
  }
  .userInfomation section {
    padding: 12px;
  }
  .userInfomation img {
    width: 40px;
  }
  .userInfomation li {
    list-style:none;
    font-size: 40px;
  }
  .userInfomation .replies,
  .userInfomation .topics {
    font-size: 0.72rem;
    border-top: 10px #DDDDDD solid;
  }
  .userInfomation > div > p {
    padding: 12px 0 12px 12px;
    background-color: rgba(212, 205, 205, 0.17);
    font-size: 0.75rem;
    margin: 0;
  }
  .userInfomation > div >ul > li {
    padding: 4px 0 4px 12px;
    white-space: nowrap;
    font-size: 0.72rem;
    text-overflow: ellipsis;
    overflow: hidden;
    line-height: 30px;
    vertical-align: middle;
  }
  .userInfomation > div >ul > li > a {
    color: #094E99;
    text-decoration: none;
  }
  .topics > ul > li > img{
    width: 30px;
  }

</style>
