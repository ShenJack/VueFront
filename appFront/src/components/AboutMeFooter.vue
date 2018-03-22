<template>
  <footer id="myFooter" class="footer">
    <div class="container">
      <div class="content has-text-centered">
        <p>
          如有任何疑问，请联系作者微信
        </p>
        <button id="button" @click="showModal" class="button is-primary modal-button" data-target="modal">
          二维码
        </button>


      </div>
      <div id="modal" class="modal" v-bind:class="{'is-active':isShow}"  @click="hideModal">
        <div class="modal-background"></div>
        <div class="modal-content">
          <p class="image is-3by3">
            <img src="../assets/wechat.jpg" alt="">
          </p>
        </div>
        <button @click="hideModal" class="modal-close is-large" aria-label="close"></button>
      </div>
    </div>

  </footer>
</template>

<script>

  export default {
    name: 'AboutMeFooter',
    data(){
      return{
        isShow:false
    }
    },
    methods: {
      showModal: function (event) {
        this.isShow = true;
      },
      hideModal: function (event) {
        this.isShow = false;
      }
    }
  }

</script>
