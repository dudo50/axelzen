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

    <p style="margin-top:20%">Do not forget to add all networks you wish to use into your Metamask. They can be added in link below by clicking metamask logo.</p>
    <a href="https://docs.axelar.dev/dev/build/contract-addresses/testnet">Axelar docs</a>

  </div>
</template>

<script lang="ts">
import { defineComponent } from '@vue/composition-api'
import { Contract, ethers, getDefaultProvider, providers } from 'ethers';
import { AxelarAssetTransfer, CHAINS, AxelarQueryAPI, Environment, EvmChain, GasToken } from '@axelar-network/axelarjs-sdk';
import web3 from 'web3'
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
      sumWfee: 0 as unknown as BigInt,
      addr: "" as string,
    };
  },
  mounted: async function () {
    this.items.push("Moonbeam")
    this.items.push("Avalanche")
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

        async valadd(val1: number, val2: number){
          this.sumWfee = BigInt(val1)+ BigInt(val2)
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
              //Magic
              var axlgateway = ""
              var axlgas = ""
              if (this.key== "Moonbeam"){
                axlgateway = "0x5769D84DD62a6fD969856c75c7D321b84d455929"
                axlgas = "0xbE406F0189A0B4cf3A05C286473D23791Dd44Cc6"
              }
              else if(this.key == "Aurora"){
                axlgateway = "0x304acf330bbE08d1e512eefaa92F6a57871fD895"
                axlgas = "0xA2C84547Db9732B27D45c06218DDAEFcc71e452D"
              }
              else if(this.key == "Avalanche"){
                axlgateway = "0xC249632c2D40b9001FE907806902f63038B737Ab"
                axlgas = "0xbE406F0189A0B4cf3A05C286473D23791Dd44Cc6"
              }
              else if(this.key == "Binance"){
                axlgateway = "0x4D147dCb984e6affEEC47e44293DA442580A3Ec0"
                axlgas = "0xbE406F0189A0B4cf3A05C286473D23791Dd44Cc6"
              }
              else if(this.key == "Fantom"){
                axlgateway = "0x97837985Ec0494E7b9C71f5D3f9250188477ae14"
                axlgas = "0xbE406F0189A0B4cf3A05C286473D23791Dd44Cc6"
              }
              else if(this.key == "Polygon"){
                axlgateway = "0xBF62ef1486468a6bd26Dd669C06db43dEd5B849B"
                axlgas = "0xbE406F0189A0B4cf3A05C286473D23791Dd44Cc6"
              }
              else if(this.key == "Ethereum"){
                axlgateway = "0xe432150cce91c13a887f7D836923d5597adD8E31"
                axlgas = "0xbE406F0189A0B4cf3A05C286473D23791Dd44Cc6"                
              }
              else{
                this.$notify({ text: `How have you even got there!?.`, type: 'error', duration: 10000,speed: 100})
                return
              }

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
            var chainFrom = CHAINS.TESTNET.MOONBEAM
            var chainTo = CHAINS.TESTNET.AVALANCHE


            //SENDER CHAIN
            if(this.key == "Moonbeam"){
              chainFrom = CHAINS.TESTNET.MOONBEAM
            }
            else if(this.key == "Avalanche"){
              chainFrom = CHAINS.TESTNET.AVALANCHE
            }
            else if(this.key == "Polygon"){
              chainFrom = CHAINS.TESTNET.POLYGON
            }
            else if(this.key == "Fantom"){
              chainFrom = CHAINS.TESTNET.FANTOM
            }
            else if(this.key == "Ethereum"){
              chainFrom = CHAINS.TESTNET.ETHEREUM
            }

            //DESTINATION CHAIN
            if(this.keyy == "Moonbeam"){
              chainTo = CHAINS.TESTNET.MOONBEAM
            }
            else if(this.keyy == "Avalanche"){
              chainTo = CHAINS.TESTNET.AVALANCHE
            }
            else if(this.keyy == "Polygon"){
              chainTo = CHAINS.TESTNET.POLYGON
            }
            else if(this.keyy == "Fantom"){
              chainTo = CHAINS.TESTNET.FANTOM
            }
            else if(this.keyy == "Ethereum"){
              chainTo = CHAINS.TESTNET.ETHEREUM
            }


            const fee = await axelarQuery.getTransferFee(
              chainFrom,
              chainTo,
              this.token,
              this.sum
            );
            this.fee = fee
            this.valadd(this.sum, this.fee.fee.amount)
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

