<template>
  <f7-page name="exchangerates">
    <f7-navbar title="Currency Exchange Rates" back-link="Back"></f7-navbar>
    <f7-block-title>Currency Exchange Rates</f7-block-title>
    <f7-block strong>
      <p>{{currentDataStatus}}</p>

      <f7-row style="text-align: center;"  v-if="displayTable !== false">
        <f7-col  width="50">
          <p>Currency</p>
        </f7-col>
        <f7-col  width="50">
          <p> 1 BTC</p>
        </f7-col>
      </f7-row>

      <f7-block style="text-align: center;" v-if="displayTable !== false">
      <f7-row  v-for="(value, key) in currencyData" :key="key">
        <f7-col  width="50">
          <p><b>{{value.code}}</b></p>
        </f7-col>
        <f7-col  width="50">
          <p>{{value.rate}}</p>
        </f7-col>
      </f7-row>
      </f7-block>

    </f7-block>
    <f7-block>
      <f7-row>
        <f7-col  width="100">
          <f7-button  fill raised @click="getdata($f7route.route.options.props.axios)">Fetch Data</f7-button>
        </f7-col>
      </f7-row>
    </f7-block>
  </f7-page>
</template>

<script>

  export default {
    data(){
      return{
        displayTable:false,
        currentDataStatus: 'No data yet',
          currencyData: '',
      }
    },
    methods: {
      getdata:function(axios) {
        this.openIndicator(true);
        this.displayTable = false;
        axios.get('https://api.coindesk.com/v1/bpi/currentprice.json')
              .then(response => {

                this.openIndicator(false);
                this.currencyData = response.data.bpi;
                this.currentDataStatus = null;
                this.displayTable = true;

              }).catch( error => {
                this.openIndicator(false);
                this.currentDataStatus = error;
                this.displayTable = false;
            });
      },

      openIndicator: function(param){
        const self = this;
        param == true ? self.$f7.preloader.show() : self.$f7.preloader.hide();
      },
    },
  }

</script>
