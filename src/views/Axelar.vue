<template>
  <div>
    <b-field class="textt" label-position="inside" label="Select origin chain">
      <b-select v-model="key" expanded placeholder="Select" required>
        <option v-for="(item) in items" :key="item">{{item}}</option>
    </b-select>
    </b-field>
    <b-field class="textt" label-position="inside" label="Select destination chain">
      <b-select v-model="keyy" expanded placeholder="Select" required>
        <option v-for="(item) in items" :key="item">{{item}}</option>
      </b-select>
    </b-field>
    <b-field class="textt" label-position="inside" label="Input destination address">
        <b-input expanded @input.native="addrs($event)" v-model="addr"></b-input>
    </b-field>
    <b-field class="textt" label-position="inside" label="Select asset you wish to transfer cross-chain">
      <b-select v-model="token" expanded placeholder="Select" required>
        <option v-for="(token) in tokens" :key="token">{{token}}</option>
      </b-select>
    </b-field>
    <b-field class="textt" label-position="inside" label="Insert token amount">
        <b-input expanded @input.native="summ($event)" v-model="sum"></b-input>
    </b-field>
    <b-button expanded @click="transactGMP()" style="margin-bottom: 1%" label="Transfer assets"></b-button>
    <b-button @click="calcSum()" expanded label="Calculate fees"></b-button>

    <p v-if="(fee!=0)" class="undername">Fee for this cross-chain transfer: {{(fee.fee.amount)}}</p>
    <p v-if="(fee!=0)" class="undername">Total transaction cost with your amount: {{sumWfee}}</p>


  </div>
</template>

<script lang="ts">
import { defineComponent } from '@vue/composition-api'
import web3 from 'web3'
import {
  AxelarQueryAPI,
  CHAINS,
  Environment,
} from "@axelar-network/axelarjs-sdk";
export default defineComponent({
  data() {
    return {
      items: [] as Array<string>,   //Stores Parachains connected to Relay chain
      tokens: [] as Array<string>,
      token: "" as string,
      keyy: "" as String,
      key: "" as String,
      sum: 0 as number,
      fee: 0 as any,
      sumWfee: 0 as number,
      addr: "" as string,
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
  methods: {
        // eslint-disable-next-line 
        async summ(value: any){
          this.sum=value.target.value
        },
        // eslint-disable-next-line 
        async addrs(value: any){
          this.addr=value.target.value
        },

        async transactGMP(){
          var err= 0;
          if(this.key=="" || this.keyy=="" || this.token=="" || this.sum==0 || this.addr=="")
          {
            this.$notify({ text: 'You did not specify details required!', type: 'error', duration: 4000,speed: 100})
          }
          else{
            //Verify addr
            try {
              const address = web3.utils.toChecksumAddress(this.addr)
            } catch(e) { 
              this.$notify({ text: `Address you provided is not valid ETH style address.`, type: 'error', duration: 4000,speed: 100})
              err = 1;
            }
            if (err == 0 ){
              console.log("HI")
            } 
            else{
              this.$notify({ text: `Your transfer was unable to be processed due to some issue, check details you provided.`, type: 'error', duration: 10000,speed: 100})
            }

          }
        },

        async calcSum(){
          if(this.key=="" || this.keyy=="" || this.token=="" || this.sum==0){
            this.$notify({ text: 'You did not specify details required!', type: 'error', duration: 4000,speed: 100})
          }
          else{
            this.$notify({ text: 'Your estimate is generating!', type:"success", duration: 4000,speed: 100})
            const axelarQuery = new AxelarQueryAPI({
            environment: Environment.TESTNET,
            });

            const fee = await axelarQuery.getTransferFee(
              CHAINS.TESTNET.MOONBEAM,
              CHAINS.TESTNET.AVALANCHE,
              this.token,
              this.sum
            );
            this.fee = fee
            this.sumWfee = await Number(this.sum)+ await Number(this.fee.fee.amount) 
            console.log(this.sumWfee, Number(this.sum), Number(this.fee.fee.amount)  )
          }
        }
  }
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

