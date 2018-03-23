<template>
  <div id="app">
    <Profile ref="myProfile" v-on:editProfile="openProfileForm"></Profile>
    <ProfileEditor ref="editor"></ProfileEditor>
    <router-view>
    </router-view>
    <AnswerCard ref="answerCard" v-show="doAnswer"></AnswerCard>
    <HangOnCard ref="hangOnCard" v-show="doHangOn"></HangOnCard>
    <Footer></Footer>
  </div>
</template>

<script>
  import Footer from './components/AboutMeFooter.vue'
  import HangOnCard from './components/HangOn.vue'
  import AnswerCard from './components/Answer.vue'
  import Profile from './components/Profile.vue'
  import ProfileEditor from './components/EditProfile.vue'

  var websocket;

  const TYPE_ERROR = 'error';
  const TYPE_NOTIFICATION = 'notification';
  const TYPE_LOG = 'log';
  const TYPE_VERIFY = 'verify';

  const TASK_ANSWER = 'answer_task';
  const TASK_HANGON = 'hang_on_task';

  export default {
    name: 'app',
    components: {
      Profile,
      HangOnCard,
      AnswerCard,
      Footer,
      ProfileEditor,
    },
    data() {
      return {
        username: '',
        password: '',
        doAnswer: false,
        doHangOn: false,
      }
    },
    methods: {
      openProfileForm(edit) {
        if (edit) {
          this.$refs.editor.show(edit);
        }
      },
      setProfile: function (data) {
        console.log(data.signed);
        this.username = data.username;
        this.password = data.password;
        this.openSocket();
        if (data.signed) {
          this.$refs.myProfile.setProfile(data)
        } else {
          this.$refs.editor.show(false);
        }
      },
      setComponents: function (data) {
        this.doAnswer = data.doAnswer;
        if(this.doAnswer){
          this.$refs.answerCard.setData(data)
        }
        this.doHangOn = data.doHangOn;
        if(this.doHangOn){
          this.$refs.hangOnCard.setData(data)
        }
      },
      initProfile() {
        const outer = this;
        this.$axios.get('/getHome')
          .then(function (response) {
            const data = response.data;
            outer.setProfile(data);
            outer.setComponents(data);
          })
      },
      openSocket: function () {
        const outer = this;
        websocket = new WebSocket('ws://127.0.0.1:8000/');
        websocket.onopen = function () {
          this.send(JSON.stringify({
            type: 'verify',
            username: outer.username,
            password: outer.password
          }));
          console.log('123');
        };
        websocket.onmessage = function (message) {
          console.log('received' + message.data);
          const object = JSON.parse(message.data);
          const content = object.content;
          switch (object.type){
            case TYPE_LOG:
              if(content.task === TASK_ANSWER){
                outer.$refs.answerCard.addLog({
                  'time':content.time,
                  'content':content.content,
                  'percent':content.percent,
                })
              }
              else if(content.task === TASK_HANGON){
                outer.$refs.hangOnCard.addLog({
                  'time':content.time,
                  'content':content.content,
                  'percent':content.percent,
                })
              }
              break;
            case TYPE_NOTIFICATION:
              alert(content)
          }
        };
        websocket.onclose = function () {
          console.log('closed')
        }
      },
    },
    created: function () {
      this.initProfile();
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
