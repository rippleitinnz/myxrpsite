<template>
  <div class="container-fluid">
    <div class="header">
      <div class="row" style="margin-right: 5px;">
        <div class="smallbig" style="width: 48.5%;">
          <div>
            <div>rippleitin.nz &nbsp;</div>
          </div>

          <div>
            <div class="smallleft">
              <div>
                Built with
                <a
                  class="ripple"
                  href="https://developers.ripple.com/build-run-rippled-ubuntu.html"
                  target="_blank"
                  style="text-decoration:none;"
                  title="Learn how to build a Rippled server"
                >RIPPLED</a> version
                <span
                  class="ripple"
                  v-if="serverState !== null"
                >{{ serverState.build_version }}</span>
              </div>
            </div>
          </div>
        </div>
        <!--   -->
        <div style="font-size: 1rem; align: center; margin-top: 5px;">
          <a
            href="https://twitter.com/rippleitinNZ"
            target="_blank"
            style="text-decoration:none;"
            class="icoTwitter"
            title="find us on Twitter"
          >
            <i class="fab fa-twitter"></i>
          </a>
        </div>

        <div class="col">
          <div class="smallest" align="right">
            <span>
              <p v-if="cryptos.XRP" style="color: black;">
                Live
                <a style="color: rgb(56, 86, 141);">XRP&nbsp;</a>prices
                <span
                  style="color: black;"
                  v-for="(rate, coin) in cryptos.XRP"
                  v-bind:key="`COINCOIN-${coin}`"
                >&nbsp; &nbsp; {{ rate }}&thinsp;{{ coin }}</span>
              </p>
              <span v-else>Loading</span>
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

let apiInterval;

export default {
  name: "XRP",

  data: function() {
    return {
      //cryptoprices
      cryptos: {
        XRP: {}
      },
      //Rawdata from my validator
      serverState: null
    };
  },

  mounted() {
    this.fetchRates();
    // Set interval at 20 seconds (re-run)
    apiInterval = setInterval(this.fetchRates, 20 * 1000);

    // Connect to server status
    this.serverStateHandler();
  },

  //end of mounted
  destroyed() {
    clearInterval(apiInterval);
  },

  //------------------------------------------------
  methods: {
    //rippled data from my server
    serverStateHandler() {
      const s = new WebSocket("wss://rippleitin.nz");
      s.onmessage = m => {
        this.serverState = JSON.parse(m.data);
      };
    },
    //rates data
    fetchRates() {
      axios
        .get(
          "https://min-api.cryptocompare.com/data/pricemulti?fsyms=XRP&tsyms=USD,EUR,AUD,NZD"
        )
        .then(response => {
          this.cryptos = response.data;
          window.console.log(response);
        })
        .catch(e => {
          window.console.log(e);
        });
    }
  }

  //end of methods
};
</script>


<style lang="scss">
#app {
  @import url(https://fonts.googleapis.com/css?family=Avenir);

  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: black;
  align: center;
}

.container-fluid {
  padding: 0 0 0 0;
}

.Tab4 {
  color: black;
  font-size: 11px;
  padding: 0 0 0 0;
}

.card-header {
  padding: 0.5rem 0.3re 0 0.3rem;
}
.darkgreen {
  color: black;
  display: flex;
  justify-content: left;
  align-items: middle;
  margin-left: 30px;
  align: center;
}
.card .btn-link {
  color: black;
}
.card .btn-link:hover {
  color: #336699;
}

.card {
  margin-bottom: 2px;
  border: 1px solid rgb(202, 202, 202);
}



.photo {
  background: url("../assets/fishing.jpg");
  background-size: 900px;
}
.nav-link:hover {
  color: red;
}
.nav-link {
  color: black;
}
.picright {
  float: right;
  margin-top: -5px;
  margin-bottom: -5px;
}

.table {
  border-left: 1px solid rgb(202, 202, 202);
  border-right: 1px solid rgb(202, 202, 202);
}
.table-striped > tbody > tr:nth-child(even) > td,
.table-striped > tbody > tr:nth-child(even) > th {
  background-color: white;
}

.table-striped > tbody > tr:nth-child(odd) > td,
.table-striped > tbody > tr:nth-child(odd) > th {
  background-color: whitesmoke;
}
.colorrow {
  background-attachment: fixed;
  border-top: solid 1px #000;
  border-bottom: solid 1px #000;
}
.colorrow2 {
  background-attachment: fixed;
  border-top: solid 1px #000;
  border-bottom: solid 1px #000;
}
.lead {
  font-size: 1.25rem;
  font-weight: 300;
  color: #6c757d !important;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  margin-top: 0.5rem;
  margin-bottom: 1rem;
}

.my-4 {
  margin-top: 1.5rem !important;
  margin-bottom: 1.5rem !important;
}
.ripple:hover {
  color: black;
}
.ripple {
  color: rgb(56, 86, 141);
}
.jumbotron {
  width: 100%;
  height: 275px;
  margin-top: -4px;
  border-top: solid 1px black;
  border-bottom: solid 1px black;
  border-radius: 0 !important;
}

h1,
h2 {
  font-weight: normal;
}

.price {
  position: fixed;
  padding: 0 0 0 0;
  font-size: 14px;
}
ul {
  list-style-type: none;
  padding: 0;
}
.margtop {
  margin-top: -60px;
}
.flag {
  text-align: left;
  padding: 0 0 0 0;
  margin-top: 5px;
  margin-left: -15px;
}

.image {
  margin-top: -8px;
  padding: 0 0 5px 0;
}

.small {
  font-size: 12px;
  margin-left: 30px;

  margin-top: 10px;
  display: flex;
  justify-content: center;
  align-items: middle;
}

.smallest {
  font-size: 12px;
  margin-top: 11px;
  margin-right: 20px;
}

.smallbig {
  font-size: 24px;
  margin-left: 36px;
  display: flex;
  align-items: flex-start;
}

.smallleft {
  font-size: 12px;
  margin-top: 11px;
  margin-left: 5px;
}
.smallbutton {
  font-size: 12px;
  margin-left: 0;
  margin-right: 0;
  display: flex;
  justify-content: center;
  align-items: middle;
}

li {
  display: inline-block;
}

table tr {
  font-size: 13px;
  line-height: 10px;
  padding: 0 0 0 0;
}
.table td {
  vertical-align: middle;
}

div.scroller {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-size: 13px;
  align: left;
  color: #000;
}
span.cursor {
  display: inline-block;
  position: relative;
  left: -7px;
  color: #666;
  top: 0.05em;
}

.serverstate {
  font-size: 14px;
}

.rate {
  margin-left: 0px;
}
.serverst {
  color: #235281;
  padding: 2px 0 3.75px 0px;
  margin-left: -35px;
}
.serverrt {
  color: black;
  padding: 2px 0 3.75px 0px;
  margin-left: 25px;
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

.hidden {
  opacity: 0;
}

.footer {
  height: 40px;
  font-size: 13px;
  bottom: 0;
  width: 100%;
  color: #235281;
  position: fixed;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 4%;
  background-color: white;
}
.footerleft {
  align-items: flex-start;
}

.header {
  height: 90px;
  // background-color: rgb(98, 189, 219);
  background-image: linear-gradient(rgb(98, 189, 219) -90%, white);
  font-size: 13px;
  top: 0;
  color: #000;
  position: inherit;
  margin-bottom: 6%;
  //border-top: solid #235281 0.75px;
  // border-bottom: solid #235281 0.75px;
  left: 50%;
}

.footerleft {
  text-align: flex-start;
}
.btn-primary {
  color: #7300e6;
  background-color: #7cc;
  border-color: #400080;
}

.btn-primary:hover {
  color: #7300e6;
  background-color: #52bebe;
  border-color: #8ad3d3;
}

.btn-primary:focus,
.btn-primary.focus {
  border-color: transparent !important;
}

.btn-primary.disabled,
.btn-primary:disabled {
  color: #7300e6;
  background-color: #7cc;
  border-color: #400080;
}

.btn-primary:not(:disabled):not(.disabled):active,
.btn-primary:not(:disabled):not(.disabled).active,
.show > .btn-primary.dropdown-toggle {
  color: #7300e6;
  background-color: #9cdada;
  border-color: #2e7c7c;
}

.btn-primary:not(:disabled):not(.disabled):active:focus,
.btn-primary:not(:disabled):not(.disabled).active:focus,
.show > .btn-primary.dropdown-toggle:focus {
  border-color: transparent !important;
}

.btn-outline-primary {
  color: z;
  background-color: transparent;
  background-image: none;
  border-color: black;
}

.btn-outline-primary:hover {
  color: #6c757d;
  border-color: black;
}

.btn-outline-primary:focus,
.btn-outline-primary.focus {
  border-color: transparent !important;
}

.btn-outline-primary.disabled,
.btn-outline-primary:disabled {
  color: #7cc;
  background-color: transparent;
}

.btn-outline-primary:not(:disabled):not(.disabled):active,
.btn-outline-primary:not(:disabled):not(.disabled).active,
.show > .btn-outline-primary.dropdown-toggle {
  color: #7300e6;
  background-color: #8ad3d3;
  border-color: #7cc;
}

.btn-outline-primary:not(:disabled):not(.disabled):active:focus,
.btn-outline-primary:not(:disabled):not(.disabled).active:focus,
.show > .btn-outline-primary.dropdown-toggle:focus {
  border-color: transparent !important;
}

.btn:focus,
.btn:active,
.btn.active,
.btn:focus:active {
  outline: none;
  -webkit-box-shadow: none;
  box-shadow: none;
}

.btn-outline-primary {
  font-size: 10px;
}

.nav-tabs .nav-link:not(.active) {
  border-color: transparent !important;
}

.nav-tabs .nav-item.show .nav-link,
.nav-tabs .nav-link.active {
  color: rgb(98, 189, 219);
}
//
</style>