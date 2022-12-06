<template>
  <div id="app">
    <b-navbar style="margin-bottom: 5%;">
      <template #start>
        <h1 style="margin-top: 1% ;" class="name" >A</h1>
        <img class="mainLogo" src="./assets/x.png" />
        <h1 style="margin-top: 1% ;margin-right: 20%;" class="name" >elZen</h1>
        <b-navbar-item  class="top undername" tag="router-link" to="/" type="is-link">Home</b-navbar-item>
        <b-navbar-item class="top undername" tag="router-link" to="/axl" type="is-link">GMP Transfer</b-navbar-item>
      </template>
      <template #end>
        <vue-metamask ref="metamask" :initConnect="false"></vue-metamask>
        
        <b-button size="is-large" type="is-text" class="namee" @click="connect">{{name}}<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/36/MetaMask_Fox.svg/2048px-MetaMask_Fox.svg.png"  width="45" height="45"></b-button>
      </template>
    </b-navbar>
    <div id="demo">
        <vue-metamask 
            userMessage="msg" 
            @onComplete="onComplete"
        >
        </vue-metamask>
    </div>
    <router-view/>
    <notifications/>

  </div>
  
</template>

<script>
import { defineComponent } from '@vue/composition-api'
import VueMetamask from 'vue-metamask';

export default defineComponent({
  components: {
            VueMetamask,
  },
  data() {
    return {
      name: "",

    };
  },
  mounted: async function () {
    this.items.push("Moonbeam")
    this.items.push("Aurora")
    this.items.push("Avalanche")
    this.items.push("Binance")
    this.items.push("Fantom")
    this.items.push("Polygon")
    this.items.push("Ethereum")

    this.tokens.push("uaxl")
    this.tokens.push("wmatic-wei")
    this.tokens.push("wftm-wei")
    this.tokens.push("eth-wei")
    this.tokens.push("wavax-wei")
    this.tokens.push("wdev-wei")
    this.tokens.push("uausdc")
    this.tokens.push("wbnb-wei")
  },
  methods:{
    onComplete(data){
      this.$notify({ text: 'Your wallet is connected!', type:"success", duration: 4000,speed: 100})
      console.log('data:', data);
      this.name = data.metaMaskAddress.slice(0,8)+"..";
    
    },
    connect() {
      this.$refs.metamask.init();

    }
  },
})

</script>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Zen+Dots&display=swap');
  @import url('https://fonts.googleapis.com/css2?family=Orbitron&display=swap');
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    background-color: white;
    margin-top: 20px;
    margin-left: 20%;
    margin-right: 20%;
  }
  .mainLogo {
    width: 10%;
  }
  .name {
    color: black;
    font-family: 'Zen Dots', cursive;
    font-size: 40px;
  }
  .namee {
    color: black;
    font-family: 'Zen Dots', cursive;
    font-size: 20px;
  }
  .undername {
    color: black;
    font-family: 'Orbitron', sans-serif;
    font-size: 16px;

  }
  .together {
    display:block
  }
  .flex__container {
    display: flex;
    flex-direction: column;
  }
  .top {
    margin-top: 8px;
  }
</style>

