<template>
  <div class="hello">
    <div>
      <mu-list>
        <mu-sub-header>最近聊天记录</mu-sub-header>
        <mu-list-item title="聊天室1" @click="chatwindow('room1')">
          <mu-avatar src="./static/img/1.jpg" slot="leftAvatar"/>
          <mu-icon value="chat_bubble" slot="right"/>
        </mu-list-item>
        <mu-list-item title="聊天室2" @click="chatwindow('room2')">
          <mu-avatar src="./static/img/2.jpg" slot="leftAvatar"/>
          <mu-icon value="chat_bubble" slot="right"/>
        </mu-list-item>
      </mu-list>
      <mu-divider/>
      <mu-list>
        <mu-sub-header>历史聊天记录</mu-sub-header>
        <mu-list-item title="聊天室1" @click="chatHistory('room1')">
          <mu-avatar src="./static/img/6.jpg" slot="leftAvatar"/>
          <mu-icon value="chat_bubble" slot="right"/>
        </mu-list-item>
        <mu-list-item title="聊天室2" @click="chatHistory('room2')">
          <mu-avatar src="./static/img/7.jpg" slot="leftAvatar"/>
          <mu-icon value="chat_bubble" slot="right"/>
        </mu-list-item>
      </mu-list>
    </div>
  </div>
</template>

<script>
  import {mapGetters} from 'vuex'
  import io from 'socket.io-client'
  import Confirm from '../components/Confirm'
  import { getItem } from '../utils/localStorage.js'

  export default {
    created() {
      if (!this.getSocket) {
        if (process.env.NODE_ENV === 'development') {
          this.$store.commit('setGetSocket', io.connect('localhost:9090/'))
        } else {
          this.$store.commit('setGetSocket', io.connect('https://github.com/holab4/myWebChat.git'))
        }
      }
    },
    async mounted() {
      const uerId = getItem('userid')
      if (!uerId) {
        await Confirm({
          title: '提示',
          content: '请先登录'
        })
        this.$router.push({ path: 'login' })
      } else {
        this.$store.commit('setTab', true)
      }
    },
    computed: {
      ...mapGetters([
        'getSocket'
      ])
    },
    methods: {
      chatHistory(roomID) {
        this.$store.commit('setTab', false)
        this.$router.push({path: '/chat-history', query: {roomId: roomID}})
      },
      chatwindow(roomID) {
        this.$store.commit('setTab', false)
        this.$router.push({path: '/chat', query: {roomId: roomID}})
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus" rel="stylesheet/stylus">

</style>
