<template>
  <transition name="fade">
    <div class="modal is-active" v-show="isShow">
      <div class="modal-background">
      </div>
      <div class="modal-card">
        <header class="modal-card-header" style="background-color: whitesmoke;padding: 10px">
          <p class="modal-card-title">{{title}} </p>
        </header>
        <section class="modal-card-body">
          <div class="field">
            <label class="label">怎么称呼？（方便辨认）<span style="color: red">*</span></label>
            <div class="control">
              <input class="input" type="text" placeholder="随便输入一个名字" v-model="name"
                     v-bind:class="{'is-danger':nameError}">
              <p class="help is-danger" v-show="nameError">有误</p>

            </div>

          </div>

          <!--<div class="field">-->
          <!--<label class="label">Unipus用户名（学号）<span style="color: red">*</span></label>-->
          <!--<div class="control has-icons-left has-icons-right">-->
          <!--<input class="input" type="number" placeholder="用户名（学号）" value="bulma" v-model="username"-->
          <!--v-bind:class="{'is-danger':usernameError}">-->
          <!--<span class="icon is-small is-left">-->
          <!--<svg class="svg-inline&#45;&#45;fa fa-user fa-w-16" aria-hidden="true" data-prefix="fas" data-icon="user" role="img"-->
          <!--xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" data-fa-i2svg=""><path fill="currentColor"-->
          <!--d="M256 0c88.366 0 160 71.634 160 160s-71.634 160-160 160S96 248.366 96 160 167.634 0 256 0zm183.283 333.821l-71.313-17.828c-74.923 53.89-165.738 41.864-223.94 0l-71.313 17.828C29.981 344.505 0 382.903 0 426.955V464c0 26.51 21.49 48 48 48h416c26.51 0 48-21.49 48-48v-37.045c0-44.052-29.981-82.45-72.717-93.134z"></path></svg>-->
          <!--</span>-->
          <!--<p class="help is-danger" v-show="usernameError">有误</p>-->
          <!--</div>-->
          <!--</div>-->

          <div class="field">
            <label class="label">密码<span style="color: red">*</span></label>
            <div class="control has-icons-left has-icons-right">
              <input class="input" type="password" placeholder="Unipus密码" value="hello@" v-model="password"
                     v-bind:class="{'is-danger':passwordError}">
              <span class="icon is-small is-left">
            <svg class="svg-inline--fa fa-lock fa-w-14" aria-hidden="true" data-prefix="fas" data-icon="lock" role="img"
                 xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512" data-fa-i2svg=""><path fill="currentColor"
                                                                                                 d="M400 224h-24v-72C376 68.2 307.8 0 224 0S72 68.2 72 152v72H48c-26.5 0-48 21.5-48 48v192c0 26.5 21.5 48 48 48h352c26.5 0 48-21.5 48-48V272c0-26.5-21.5-48-48-48zm-104 0H152v-72c0-39.7 32.3-72 72-72s72 32.3 72 72v72z"></path></svg>

    </span>
              <p class="help is-danger" v-show="passwordError">有误</p>
            </div>
          </div>

          <div class="columns">
            <div class="field column">
              <label class="label" style="text-align: left">年级<span style="color: red">*</span></label>

              <div class="control">
                <div class="select" v-bind:class="{'is-danger':gradeError}">
                  <select v-model="grade">
                    <option disabled value="">请选择</option>
                    <option>大二下</option>
                    <option>大一下</option>
                  </select>
                </div>
              </div>
              <p class="help is-danger" v-show="gradeError">有误</p>

            </div>
            <div class="field column">
              <label class="label" style="text-align: left" v-bind:class="{'is-danger':codeError}">填写验证码了没？<span
                style="color: red">*</span></label>

              <div class="control">
                <div class="select" v-bind:class="{'is-danger':codeError}">
                  <select v-model="hasEnteredCode">
                    <option disabled value="">请选择</option>
                    <option>填了</option>
                    <option>没填</option>
                    <option>什么玩意儿？</option>
                  </select>
                </div>
                <p class="help is-danger" v-show="codeError">有误</p>

              </div>

            </div>
            <div class="field column">

            </div>
          </div>

          <div style="padding: 2px 2px 1px 2px;" v-bind:style="{background:taskContentErrorColor}">
            <div style="background-color: white">
              <div class="field">
                <div class="control">
                  <label class="checkbox">
                    <input type="checkbox" v-model="enableScoreInput">
                    刷题（在后面附上要刷的大概分数，0-100分）
                  </label>
                </div>
              </div>
              <div class="field">
                <div class="control">
                  <input v-bind:class="{'is-danger':scoreError}" v-model="score" :disabled="!enableScoreInput"
                         class="input is-small" type="number"
                         placeholder="分数">
                  <p class="help is-danger" v-show="scoreError">有误</p>

                </div>
              </div>

              <div class="field">
                <div class="control">
                  <label class="checkbox">
                    <input type="checkbox" v-model="enableHangOnInput">
                    挂机（在后面附上要挂的时间，0-800 <span style="color: red">分钟</span>）
                  </label>
                </div>
              </div>
              <div class="field">
                <div class="control">
                  <input v-model="hangOnTime" v-bind:class="{'is-danger':timeError}" :disabled="!enableHangOnInput"
                         class="input is-small" type="number" placeholder="时间，单位：分钟">
                </div>
              </div>
              <p class="help is-danger" v-show="timeError">有误</p>
            </div>
          </div>
          <p class="help is-danger" v-show="taskContentError">请至少选择一个</p>

          <div class="field">
            <label class="label">联系方式?过程出了问题，方便我联系到你</label>
            <div class="control">
              <input v-model="contact" class="input is-small" type="text" placeholder="任何联系方式都行">
            </div>
          </div>

          <div class="field">
            <label class="label">备注</label>
            <div class="control">
              <textarea v-model="remarks" class="textarea" placeholder="有啥想嘱咐我的，我一定能看到的"></textarea>
            </div>
          </div>


          <div class="field is-grouped">
            <div class="control">
              <button class="button is-link" @click="checkAndSend">提交</button>
            </div>
            <div class="control">
              <button @click="cancel" class="button is-text" v-show="cancelable">取消</button>
            </div>
          </div>

        </section>
        >
      </div>
    </div>
  </transition>
</template>

<script>
  import Vue from 'vue'

  export default {
    name: 'editProfile',
    data() {
      return {
        name: '',
        username: '',
        password: '',
        grade: '',
        hasEnteredCode: '',
        enableScoreInput: false,
        enableHangOnInput: false,
        cancelable: false,

        hangOnTime: '0',
        score: '0',

        isShow: false,

        contact: '',
        remarks: '',

        // usernameError: true,
        // passwordError: true,
        // gradeError: true,
        // codeError: true,
      }
    },
    computed: {
      usernameError: function () {
        return this.username.length !== 9;
      },
      passwordError: function () {
        return this.password === '';
      },
      gradeError: function () {
        return this.grade === '';
      },
      codeError: function () {
        return this.hasEnteredCode === '';
      },
      taskContentErrorColor: function () {
        if (this.taskContentError) {
          return '#ffaaae';
        } else {
          return 'white';
        }
      },
      taskContentError: function () {
        return !this.enableHangOnInput && !this.enableScoreInput;
      },
      timeError: function () {
        return this.enableHangOnInput && (this.hangOnTime <= 0 || this.hangOnTime > 800);
      },
      scoreError: function () {
        return this.enableScoreInput && (this.score <= 0 || this.score > 100);
      },
      nameError: function () {
        return this.name === '';
      }
    },
    methods: {
      checkAndSend: function () {
        if (!this.nameError && !this.passwordError && !this.gradeError && !this.codeError && !this.taskContentError && !this.timeError && !this.scoreError) {
          alert('pass');
          this.send()
        } else {
          alert('error');
          this.send()
        }
      },
      send: function () {
        const outer = this;
        this.$axios.post('/editProfile/', require('qs').stringify({
          name: this.name,
          password: this.password,
          grade: this.grade,
          score: this.score,
          doAnswer: this.enableScoreInput,
          doHangOn: this.enableHangOnInput,
          hangOnTime: this.hangOnTime,
          hasEnteredCode: this.hasEnteredCode,
          contact: this.contact,
          remarks: this.remarks
        }))
          .then(function (response) {
            if(response.status!==200){
              alert('错误，请联系作者');
              outer.cancel();
            }else {
              outer.cancel();
            }
          })
      },
      show: function (edit) {
        this.isShow = true;
        if (edit) {
          this.title = '修改信息';
          this.cancelable = true;
        } else {
          this.title = '补全信息';
          this.cancelable = false;
        }
      },
      cancel: function () {
        this.isShow = false;
      }
    }
  }
</script>
