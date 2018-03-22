<template>
  <div class="container">
    <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          挂机<span class="tag" v-bind:class="statusLabel">{{statusText}}</span>
        </p>
      </header>
      <div class="card-content">
        <div class="columns">
          <div class="column is-2">
            <p class="title">进度</p>
          </div>
          <div class="column is-7">
            <progress style="margin-top:10px;margin-left: 10px; margin-right: 10px" class="progress is-primary"
                      :value="hangOnPercent" max="100">
            </progress>
          </div>
          <div class="column is-3">
            <p class="title">{{hangOnMinutes}}/{{totalMinutes}}分钟</p>
          </div>
        </div>

        <div class="columns">
          <div class="column is-2">
            <p style="margin-top: 12px" class="title">操作</p>
          </div>

          <div class="column">
            <a v-bind:class="{'is-loading':buttonLoading20}" @click="startHangOn(20)" :disabled="disableStart20" class="button is-warning is-large"
               style="margin-top: 5px">开始20%</a>
            <a v-bind:class="{'is-loading':buttonLoading50}" @click="startHangOn(50)" id="start50" :disabled="disableStart50" class="button is-warning is-large"
               style="margin-top: 5px">开始50%</a>
            <a v-bind:class="{'is-loading':buttonLoading100}" @click="startHangOn(100)" id="start100" :disabled="disableStart100" class="button is-warning is-large"
               style="margin-top: 5px">开始100%</a>
            <a v-bind:class="{'is-loading':buttonLoadingLeft}" @click="startHangOn(0)" id="startLeft" :disabled="disableStartLeft" class="button is-warning is-large"
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
  </div>
</template>

<script>

  const TYPE_LOG = 80;
  const TYPE_ERROR = 81;
  const TYPE_SUCCESS = 82;
  const TYPE_RUNTIME_ERROR = 83;
//  const TYPE_


  const STATUS_RUNNING = 0;
  const STATUS_IDLE = 1;
  const STATUS_SUCCESS = 2;
  export default {
    name: 'hangOn',
    data() {
      return {
        statusText: '空闲中',
        hangOnPercent: 80,
        hangOnMinutes: 670,
        totalMinutes: 800,
        statusLabel: 'is-light',
        logs: [
          {
            time: '2018-02-02',
            content: '第24页完成 分数89',
            percent: '38',
          },
        ],
        buttonLoading20:false,
        buttonLoading50:false,
        buttonLoading100:false,
        buttonLoadingLeft:false,
      }
    },
    computed: {
      disableStart20: function () {
        return this.hangOnPercent > 80;
      },
      disableStart50: function () {
        return this.hangOnPercent > 50;
      },
      disableStart100: function () {
        return this.hangOnPercent > 0;
      },
      disableStartLeft: function () {
        return this.hangOnPercent === STATUS_SUCCESS;
      }
    },

    methods: {
      addLog: function (log) {
        log = {
          time: '12314',
          content: 'kjnom',
          percent: '49'
        };
        this.logs.push(log)
      },
      setStatus: function (status) {
        switch (status) {
          case STATUS_RUNNING:
            this.statusText = '进行中';
            this.statusLabel = 'is-dark';
            break;
          case STATUS_SUCCESS:
            this.statusText = '已完成';
            this.statusLabel = 'is-success';
            break;
          case STATUS_IDLE:
            this.statusText = '空闲中';
            this.statusLabel = 'is-light';
            break;
        }
      },
      setSendingState: function (percent) {
        switch(percent){
          case 20:
            if(!this.disableStart20){
              this.buttonLoading20 = true;
            }
            break;
          case 50:
            if(!this.disableStart50){
              this.buttonLoading50 = true;
            }
            break;
          case 100:
            if(!this.disableStart100){
              this.buttonLoading100 = true;
            }
            break;
          case 0:
            if(!this.disableStartLeft){
              this.buttonLoadingLeft = true;
            }
            break;
        }
      },
      startHangOn: function (percent) {
        if (percent > 100 || percent < 0) {
//        error
        }else {
          this.setSendingState(percent)
          /*send request*/
        }
      },
      processNotification:function (notification) {
        switch(notification.type){
//          case notification
        }
      }
    },
    created: function () {
      this.addLog();
      this.setStatus(STATUS_IDLE)
    },

  }
</script>
