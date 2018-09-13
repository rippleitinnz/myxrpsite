<template>

<div id="app">
<div class="flag"><b-img width="54px" height="40px" src="src/assets/flag.gif"/>
</div>
<b-jumbotron fluid text-variant="secondary" border-variant="dark" >
<template slot="header">
<div><b-img width="384" height="107.25" src="src/assets/xrp.png"/></div>
</template>
<template slot="lead">
rippleitin.nz is an independent XRP validating node based in New Zealand
</template>
<hr class="my-4">
<b-container class="bv-example-row">
<b-row>
<b-col cols="8">
<div class="console-container" align="left">XRP
<span id='text'>
</span>
</div>
</b-col>
<b-col cols="4">
</b-col>  </b-row>  </b-container>
</b-jumbotron>

<!-- jumbotron-3.vue -->

<b-container fluid align="center">
<b-row class="justify-content-md-center">
<b-col cols="auto" class="p-4">
<div class="shadow bg-white rounded">
<b-card align="center" header="This Server's Validating Metrics" header-text-variant="dark">
<div class="serverst">
<b-row>
<b-col>Server State</b-col><b-col class="serverrt" v-if="serverState !== null" > <b><span class="text-success">{{ serverState.server_state }}</span></b>
</b-col>
</b-row>
</div>

<div class="serverst">
<b-row>
<b-col>Build Version </b-col><b-col class="serverrt" v-if="serverState !== null" > {{ serverState.build_version }}
</b-col>
</b-row>
</div>

<div class="serverst">
<b-row>
<b-col>Server Uptime </b-col><b-col class="serverrt" v-if="serverState !== null" > {{ Math.round(serverState.uptime /86400 * 100) / 100}} days
</b-col>
</b-row>
</div>

<div class="serverst">
<b-row>
<b-col>Peers</b-col> <b-col class="serverrt" v-if="serverState !== null" > {{ serverState.peers }}
</b-col>
</b-row>
</div>

<div class="serverst">
<b-row>
<b-col>Last Closed Ledger</b-col> <b-col class="serverrt"><div  v-if="serverState !== null" > <b>{{ serverState.validated_ledger.seq }}</b>
</div>
<div class="serverwait" v-else>
Awaiting server state...
</div>
</b-col>
</b-row>
</div>

<div class="serverst">
<b-row>
<b-col>Complete Ledgers</b-col> <b-col class="serverrt" v-if="serverState !== null" > {{ serverState.complete_ledgers }}
</b-col>
</b-row>
</div>



<div class="serverst">
<b-row>
<b-col>Last Ledger #</b-col> <b-col class="serverrt" v-if="serverState !== null" > {{ serverState.validated_ledger.hash }}
</b-col>
</b-row>
</div>

<div class="serverst">
<b-row>
<b-col>Public Key</b-col><b-col class="serverrt"
v-if="serverState !== null" > {{ serverState.pubkey_node }}
</b-col>
</b-row>
</div>
</b-card>
</div>
</b-col>

<b-col cols="auto" class="p-4">
<div class="shadow bg-white rounded">
<b-card align="left"
header="Live XRP Prices"
header-bg-variant="light"
header-text-variant="dark"
>
<p>
<p v-if="cryptos.XRP">
<ul v-for="(rate, coin) in cryptos.XRP" v-bind:key="coin">
{{ coin }}   {{ rate }}
</ul>
</p>
<span v-else>
Loading rates...
</span>
</p>
</b-card>
</div>
</b-col>
</b-row>

</b-container>

<!-- b-col-8.vue -->

<b-container fluid class="footer">
<b-row>
<b-col cols="12">
&#169; rippleitin.nz 2018 - new zealand's original XRP validating node
</b-col>
</b-row>
</b-container>
</div>
</template>

<script>
import axios from 'axios'
let apiInterval

export default {
name: 'XRP',
data () {
return {
cryptos: {
XRP: {}
},
serverState: null
}
},
mounted () {
// Run immediately
this.fetchRates()
// Set interval at 7 seconds (re-run)
apiInterval = setInterval(this.fetchRates, 7 * 1000)
// Connect to server status
this.serverStateHandler()
},
destroyed () {
clearInterval(apiInterval)
},
methods: {
serverStateHandler () {
const s = new WebSocket('wss://rippleitin.nz')
s.onmessage = (m) => {
this.serverState = JSON.parse(m.data)
}
},
fetchRates () {
axios.get('https://min-api.cryptocompare.com/data/pricemulti?fsyms=XRP&tsyms=USD,EUR,GBP,AUD,NZD')
.then(response => {
this.cryptos = response.data
window.console.log(response)
})
.catch(e => {
window.console.log(e)
})
}
}
}
</script>

<style lang="scss">

#app {
font-family: 'Avenir', Helvetica, Arial, sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
text-align: center;
color: #2c3e50;
margin-top: 1px;
}

.b-container {
padding: 0 0 20px 0;
}
.jumbotron{
background-image: linear-gradient(120deg, #8fd3f4 0%, #84fab0 100%);
width: 100%;
height: 315px;
}

h1, h2 {
font-weight: normal;
}

.price {
position: fixed;
padding: 0 0 0 0;
font-size: 14px;
}

.card{
padding: 0,
}
ul {
list-style-type: none;
padding: 0;
}

.flag {
text-align: left;
margin-left: 10px;
}

li {
display: inline-block;
}

a {margin-right: -10px;
color: #42b983;
}


.console-container {
font-size:16px;
align: left;
color:#000;
}
.serverstate {
font-size: 14px;
}

.serverst{
color: green;
padding: 2px 0 4.75px 0px;
text-align: left;
}
.serverrt{
color: #2c3e50;
text-align: left;
}

.ledger {
font-size: 11px;
color: orange;
}

.serverwait {
color: orange;
}

.crypto-container {
width: 100%;
height: 60px;
display: flex;
}

.footer {
height: 50px;
background-image: linear-gradient(120deg, #84fab0 0%, #8fd3f4 100%);
font-size: 13px;
color: #000;
bottom: 15px;
position: fixed;
display: flex;
justify-content: center;
align-items: center;
border-bottom: solid black 1px;
border-top: solid black 1px;
}


</style>
