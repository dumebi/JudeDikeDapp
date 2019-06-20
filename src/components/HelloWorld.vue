<template>
<div class="container">
  <div class="row">
    <div class="col-sm-4">
      <h3>Token Symbol</h3>
      <p>{{token[0]}}</p>
    </div>
    <div class="col-sm-4">
      <h3>Token Name</h3>
      <p>{{token[1]}}</p>
    </div>
    <div class="col-sm-4">
      <h3>Total Supply</h3>        
      <p>{{parseInt(token[2])}}</p>
    </div>
  </div>
  <div class="row">
    <div class="col-sm-12">
      <h3>Minter</h3>
      <p>{{minter}}</p>
    </div>
  </div>
  <div class="row justify-content-center">
    <div class="col-sm-6 ">
      <h3>Mint token</h3>
      <form>
      <div class="form-group">
        <label for="exampleInputEmail1">Amount of tokens to mint</label>
        <input v-model="amount" type="number" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" placeholder="Enter token amount">
      </div>
      <button @click="mint" :disabled="amount < 1" type="button" class="btn btn-primary">Mint</button>
    </form>
    </div>
  </div>
  <!-- <div class="row justify-content-center">
    <div class="col-sm-8 ">
      <h3>Set Minter</h3>
      <form>
      <div class="form-group">
        <label for="exampleInputEmail1">New minter address</label>
        <input v-model="newMinter" type="text" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" placeholder="Enter email">
      </div>
      <button @click="setMinter" :disabled="newMinter < 1" type="button" class="btn btn-primary">Set Minter</button>
    </form>
    </div>
  </div> -->
</div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data: function () {
    return {
      token: [],
      minter: '',
      newMinter: '',
      amount: '',
      notificationSystem: {
        options: {
          ballon: {
            position: 'topRight'
          },
          info: {
            position: 'topRight'
          },
          success: {
            position: 'topRight'
          },
          warning: {
            position: 'topRight'
          },
          error: {
            position: 'topRight'
          }
        }
      }
    }
  },
  mounted () {
    this.init()
  },
  methods: {
    async init() {
      // console.log(window.MyContract)
      try {
        const tokenDetails = await window.MyContract.methods.details().call()
        this.token = tokenDetails

        const minterDetails = await window.MyContract.methods.minter().call()
        this.minter = minterDetails
        this.$toast.success('Token details fetched', '', this.notificationSystem.options.success)
      } catch (error) {
        this.$toast.error('Error getting token details', '', this.notificationSystem.options.error)
      }
     
    },

    async mint() {
      // console.log(this.amount, this.minter)
      try {
        const minter = await window.MyContract.methods.mint(this.amount).send({
            from: this.minter,
            gas: 2000000
        })
        if(minter){
          this.$toast.success('Token has been minted', '', this.notificationSystem.options.success)
        }
        const tokenDetails = await window.MyContract.methods.details().call()

        this.token = tokenDetails
      } catch (error) {
        this.$toast.error('could not mint token', '', this.notificationSystem.options.error)
      }
     
    },

    // async setMinter() {
    //   console.log(this.amount, this.minter)
    //   try {
    //     const minter = await window.MyContract.methods.setMinter(this.newMinter).send({
    //         from: '0x77598660059c39924d068940B26E9F3fc373261A',
    //         gas: 2000000
    //     })
    //     console.log(minter)
    //     const minterDetails = await window.MyContract.methods.minter().call()
    //     this.minter = minterDetails
    //   } catch (error) {
    //     console.log(error)
    //   }
     
    // }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
