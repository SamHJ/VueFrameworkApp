<template>
  <f7-page name="signup">
    <f7-navbar title="Form" back-link="Back"></f7-navbar>

    <f7-block-title>SignUp {{param}}</f7-block-title>

    <f7-block v-if="displayMessage !== false">
      <f7-row>
        <f7-col  width="100">
    <p class="important-note" style="padding: 5px 10px 5px 15px;
    background: #fde9e5;color: #000;border-left: 4px solid #ee350f;
    transition-duration: .4s;
    position: relative;" v-if="showkind =='error'">{{message}}</p>

    <p class="important-note" style="padding: 5px 10px 5px 15px;
    background: #42b983;color: #fff;border-left: 4px solid green;
    transition-duration: .4s;
    position: relative;" v-if="showkind =='success'">{{message}}</p>
  </f7-col>
</f7-row>
</f7-block>

    <f7-list no-hairlines-md>
      <f7-list-input
        label="Name"
        type="text"
        placeholder="Your name"
        :value="name"
        @input="name = $event.target.value"
        clear-button
        floating-label
        clear-button
        outline
      ></f7-list-input>

      <f7-list-input
        label="E-mail"
        type="email"
        placeholder="E-mail"
        :value="email"
        @input="email = $event.target.value"
        clear-button
        floating-label
        clear-button
        outline
      ></f7-list-input>

      <f7-list-input
        label="Phone"
        type="text"
        placeholder="Phone"
        :value="phone"
        @input="phone = $event.target.value"
        clear-button
        floating-label
        clear-button
        outline
      ></f7-list-input>

      <f7-list-input
        label="Password"
        type="password"
        placeholder="Password"
        :value="password"
        @input="password = $event.target.value"
        clear-button
        floating-label
        clear-button
        outline
      ></f7-list-input>
    </f7-list>

    <f7-block>
      <f7-row>
        <f7-col  width="100">
    <f7-row tag="p">
      <f7-button class="col" style="width:100%;text-align:center;align-items:center"
      @click="signUp($f7route.route.options.props.axios)" raised>SignUp</f7-button>
    </f7-row>
  </f7-col>
</f7-row>
  </f7-block>

  </f7-page>
</template>

<script>

  export default {
    //receiving passed props from the home.vue file
    props: ["param"],
    data(){
      return{
         name: '',
         email:'',
         phone:'',
         password:'',
         displayMessage: false,
         message:'',
         showkind:'',
         // props:['axios']
      }
    },
    methods: {
      signUp:function(axios) {
        // console.log(param);
        this.openIndicator(true);
        this.displayMessage = false;

        if(this.isEmpty(this.name.trim()) || this.isEmpty(this.email.trim()) ||
           this.isEmpty(this.phone.trim()) || this.isEmpty(this.password.trim())){
        //input fields are empty
        this.openIndicator(false);
        this.message = "All fields are required!";
        this.showkind = "error";
        this.displayMessage = true;

      }else {
        const FormData = require('form-data');
        //input fields are not empty
      var optionAxios =  { headers:{
              'Content-Type': 'application/x-www-form-urlencoded'

        } }
        const form = new FormData();
        form.append('name', this.name.trim());
        form.append('email', this.email.trim());
        form.append('phone', this.phone.trim());
        form.append('password', this.password.trim());
        axios.post('https://greenleafstudios.com.ng/vuesignup.php/signup',form,
        optionAxios
      ).then(response => {

                this.openIndicator(false);

                if(response.data == "success"){
                  this.message = "Signup successfull!";
                  this.showkind = "success";
                  this.displayMessage = true;
                }else if (response.data == "alreadyexists") {
                  this.message = "Email already exists!";
                  this.showkind = "error";
                  this.displayMessage = true;
                }
                else {
                  this.message = "An error occurred!";
                  this.showkind = "error";
                  this.displayMessage = true;
                }

              }).catch( error => {
                this.openIndicator(false);
                this.message = error;
                this.showkind = "error";
                this.displayMessage = true;
            });
      }

      },

      openIndicator: function(param){
        const self = this;
        param == true ? self.$f7.preloader.show() : self.$f7.preloader.hide();
      },
      isEmpty: function(v){
              let type = typeof v;
              if (type === 'undefined') {
                  return true;
              }
              if (type === 'boolean') {
                  return !v;
              }
              if (v === null) {
                  return true;
              }
              if (v === undefined) {
                  return true;
              }
              if (v instanceof Array) {
                  if (v.length < 1) {
                      return true;
                  }
              } else if (type === 'string') {
                  if (v.length < 1) {
                      return true;
                  }
                  if (v === '0') {
                      return true;
                  }
              } else if (type === 'object') {
                  if (Object.keys(v).length < 1) {
                      return true;
                  }
              } else if (type === 'number') {
                  if (v === 0) {
                      return true;
                  }
              }
              return false;
      },
    },
  }

</script>
