<template>
  <div class="container">
    <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          <strong>刷题</strong><span class="tag" v-bind:class="statusLabel">{{statusText}}</span>
        </p>
      </header>
      <div class="card-content">
        <div class="columns">
          <div class="column is-2">
            <p class="title">进度</p>
          </div>
          <div class="column is-7">
            <progress style="margin-top:10px;margin-left: 10px; margin-right: 10px" class="progress is-primary"
                      :value="answerPercent" max="100">
            </progress>
          </div>
          <div class="column is-3">
            <p class="title">{{answerPercent}}%完成</p>
          </div>
        </div>

        <div class="columns">
          <div class="column is-2">
            <p style="margin-top: 12px" class="title">操作</p>
          </div>

          <div class="column">
            <a :class="{'is-loading':buttonLoading25}" @click="startAnswer(disableStart25,25)" :disabled="disableStart25"
               class="button is-warning is-large"
               style="margin-top: 5px">开始25%</a>
            <a :class="{'is-loading':buttonLoading50}" @click="startAnswer(disableStart50,50)" :disabled="disableStart50"
               class="button is-warning is-large"
               style="margin-top: 5px">开始50%</a>
            <a :class="{'is-loading':buttonLoading100}" @click="startAnswer(disableStart100,100)" :disabled="disableStart100"
               class="button is-warning is-large"
               style="margin-top: 5px">开始100%</a>
            <a :class="{'is-loading':buttonLoadingLeft}" @click="startAnswer(disableStartLeft,0)" :disabled="disableStartLeft"
               class="button is-warning is-large"
               style="margin-top: 5px">开始剩下全部</a>
          </div>

        </div>


      </div>


    </div>

    <div class="card">
      <header class="card-header">
        <p class="card-header-title">日志</p>
      </header>
      <div style="height: 300px; overflow: scroll">

        <table class="table is-bordered is-striped is-narrow is-hoverable is-fullwidth">
          <thead>
          <tr>
            <th><abbr title="发生的时间">时间</abbr></th>
            <th><abbr title="日志内容">内容</abbr></th>
            <th><abbr title="当时的百分比">进度</abbr></th>
          </tr>
          </thead>

          <tbody>
          <tr v-for="log in logs">
            <th>{{log.time}}</th>
            <td>{{log.content}}</td>
            <td>{{log.percent}}</td>
          </tr>
          </tbody>

        </table>
      </div>
    </div>
    <br>
  </div>
</template>

<script>
  import Vue from 'Vue'

  const STATUS_ING = "doing";
  const STATUS_OK = "success";
  const STATUS_ERROR = "error";
  const STATUS_WAIT = "wait";

  export default {
    name: 'answerCard',
    data() {
      return {
        logs: [
          {
            time: '2018-02-02',
            content: '第24页完成 分数89',
            percent: '38',
          },
        ],
        answerPercent: 50,
        statusText: '',
        statusLabel: '',
        answerStatus: '',

        buttonLoading25: false,
        buttonLoading50: false,
        buttonLoading100: false,
        buttonLoadingLeft: false,

//        disableStart20: this.answerPercent<80,
//        disableStart50: this.answerPercent<50,
//        disableStart100: this.answerPercent<=0,
      }
    },
    computed: {
      disableStart25: function () {
        return this.answerPercent > 75;
      },
      disableStart50: function () {
        return this.answerPercent > 50;
      },
      disableStart100: function () {
        return this.answerPercent > 0;
      },
      disableStartLeft: function () {
        return this.answerStatus === STATUS_OK;
      }
    },
    methods: {
      setStatus: function (status) {
        this.answerStatus = status;
        switch (status) {
          case STATUS_ING:
            this.statusText = '进行中';
            this.statusLabel = 'is-dark';
            break;
          case STATUS_OK:
            this.statusText = '已完成';
            this.statusLabel = 'is-success';
            break;
          case STATUS_WAIT:
            this.statusText = '空闲中';
            this.statusLabel = 'is-light';
            break;
          case STATUS_ERROR:
            this.statusText = '错误';
            this.statusLabel = 'is-danger'
        }
      },
      setSendingState: function (percent) {
        switch (percent) {
          case 20:
            if (!this.disableStart25) {
              this.buttonLoading25 = true;
            }
            break;
          case 50:
            if (!this.disableStart50) {
              this.buttonLoading50 = true;
            }
            break;
          case 100:
            if (!this.disableStart100) {
              this.buttonLoading100 = true;
            }
            break;
          case 0:
            if (!this.disableStartLeft) {
              this.buttonLoadingLeft = true;
            }
            break;
        }
      },
      cancelSendingState: function () {
        this.buttonLoading25 = false;
        this.buttonLoading50 = false;
        this.buttonLoading100 = false;
        this.buttonLoadingLeft = false;
      },
      startAnswer: function (disable,percent) {
        const outer = this;
        if(!disable){
          if (percent > 100 || percent < 0) {
//        error
          } else {
            this.setSendingState(percent);
            /*send request*/
            this.$axios.post('/startAnswer/', require('qs').stringify({
              percent: percent,
            })).then(function (response) {
//            alert(response.status);
              if (response.status === 200) {
                outer.setStatus(STATUS_ING);
//                outer.cancelSendingState();
                alert(response.data)
              } else {
                outer.setStatus(STATUS_ERROR);
                outer.cancelSendingState();
                alert(response.data)
              }
            })
          }

        }
      },
      addLog: function (log) {
        this.logs.unshift(log);
        this.answerPercent = log.percent;
      },
      setData: function (data) {
        this.answerPercent = data.answerPercentage;
        this.setStatus(data.answerStatus);
      }

    },
    created: function () {
//      this.answerPercent = 80;
//      this.setDisabled(this.answerPercent)
//      this.addLog();
//      this.setStatus(STATUS_SUCCESS)
    }

  }</script>
