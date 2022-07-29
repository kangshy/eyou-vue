<template>
  <div class="about">
    <h1>This is an about page...</h1><br>
    <h1>token:  {{ token }}</h1><br>
    <h1>snsId:  {{ snsId }}</h1><br>
    <h1>nickname:  {{ nickname }}</h1><br>
    <h1>profile_img:  {{ profile_img }}</h1><br>
  </div>
</template>

<script>
import axios from "axios";
import { mapGetters } from "vuex"

export default {
    name : "AboutView",

    data: () => ({
    nextUrl: "",
  }),

  created() {
    this.initialize();
  },

    computed : {  
      ...mapGetters('user', ['snsId','hasToken', 'token',"nickname", "profile_img"])
    },

    methods : {
      initialize() {
        const _this = this;

        console.log("snsId: " + _this.snsId);
        // console.log("token: " + _this.token);
        console.log("nickname: " + _this.nickname);
        console.log("profile_img: " + _this.profile_img); 

        //로그인 확인
        // const _this = this;
        if( _this.hasToken ){
          console.log("token is alive")
          console.log("token: "+_this.token)
          // 로그인 페이지로 이동

          // 로그인 성공 후 next url로 이동
          // _this.$router.push("/" + _this.nextUrl);
          
        }
    },
}
}


</script>
