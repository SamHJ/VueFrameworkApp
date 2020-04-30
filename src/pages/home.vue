<template>
  <f7-page name="home">
    <!-- Top Navbar -->
    <f7-navbar large :sliding="false">
      <f7-nav-left>
        <f7-link icon-ios="f7:menu" icon-aurora="f7:menu" icon-md="material:menu" panel-open="left"></f7-link>
      </f7-nav-left>
      <f7-nav-title sliding>VueApp</f7-nav-title>
      <f7-nav-title-large>VueApp</f7-nav-title-large>
    </f7-navbar>


    <f7-toolbar bottom tabbar labels>

   <f7-link tab-link="#tab-1" :tab-link-active="isactive1" @click="changeBottomContent('inbox')">
     <f7-icon class="icon-fill" ios="f7:envelope_fill" aurora="f7:envelope_fill" md="material:email">
       <f7-badge color="green">5</f7-badge>
     </f7-icon>
     <span class="tabbar-label">Inbox</span>
   </f7-link>

   <f7-link tab-link="#tab-2" :tab-link-active="isactive2" @click="changeBottomContent('login')">
     <f7-icon ios="f7:today" aurora="f7:today" md="material:today">
       <f7-badge color="red">7</f7-badge>
     </f7-icon>
     <span class="tabbar-label">Login</span>
   </f7-link>

   <f7-link tab-link="#tab-3" :tab-link-active="isactive3" @click="changeBottomContent('signup')">
     <f7-icon ios="f7:cloud" aurora="f7:cloud" md="material:file_upload">
       <f7-badge color="red">1</f7-badge>
     </f7-icon>
     <span class="tabbar-label">SignUp</span>
   </f7-link>

 </f7-toolbar>


       <Inbox v-if="activeBottom == 'inbox'"  :param="'Emenike Samuel'" />
       <Login v-if="activeBottom == 'login'" :param="true" />
       <SignUp v-if="activeBottom == 'signup'" :param="true" /> <!-- passing a 'param' prop to the SignUp component -->

  </f7-page>
</template>

<script>

import Inbox from './inbox.vue';
import Login from './login.vue';
import SignUp from './signup.vue';

export default{
  data(){
    return {
      activeBottom:'inbox',
      isactive1: true,
      isactive2: false,
      isactive3: false,
    }
  },
  methods:{
    changeBottomContent: function(param){
      this.activeBottom = param;
      if(param == 'inbox'){
        this.isactive1 = true;
        this.isactive2 = false;
        this.isactive3 = false;
      }else if (param == 'login') {
        this.isactive1 = false;
        this.isactive2 = true;
        this.isactive3 = false;
      }else{
        this.isactive1 = false;
        this.isactive2 = false;
        this.isactive3 = true;
      }
    }
  },
  components:{
    Inbox,
    Login,
    SignUp
  }
}

</script>
