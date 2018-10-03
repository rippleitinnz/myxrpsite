<template>
  <div id="app">

    <div class="container-fluid">
      <div class="row">
        <div class="col-2">
          <div class="flag">
            <div class="media">
              <img src="../assets/flag.gif" alt="NZ Flag" class="" height="40" width="54" />
            </div>
          </div>
        </div>

        <div class="col-8">
          <div>
            <span class="small">
              <p v-if="cryptos.XRP" style="color: #400080;">XRP Prices
                <span style="color: #7300e6;" v-for="(rate, coin) in cryptos.XRP" v-bind:key="coin">
                  &nbsp &nbsp {{ rate }}&thinsp;{{ coin }}
                </span>
              </p>
              <span v-else>
                Loading <rates class="">
                </rates>
              </span>
            </span>
          </div>
        </div>


        <div class="col-2">

          <span class="smallbutton" style="margin-top: 11px;">
            <ul class="nav justify-content-right">
              <li class="nav-item">
                <a href="https://developers.ripple.com/build-run-rippled-ubuntu.html" target="_blank" class="btn btn-outline-primary btn-xs inactive"
                  role="button" aria-pressed="true"> BUILD RIPPLED </a>
              </li>
            </ul>
          </span>
        </div>


      </div>
    </div>
    <div>
      <div class="jumbotron" text-variant="secondary" border-variant="dark" align="center">
        <div>
          <img src="../assets/xrp.png" class="image" height="87" width="307" />
        </div>
        <p class="lead">
          rippleitin.nz is an independent XRP validating node based in New Zealand
        </p>


        <div class="container">
          <hr class="my-4" style="width: 100%;">
          <div class="row">
            <div class="col-8">

              <div class="console-container" align="left">XRP
                <span id='text'></span>
                <div class="console-underscore" id="console">|</div>
              </div>
            </div>
            <div class="col-4">
              <div align="right" style="color: black; font-size: 13px;">
                Built with <a class="ripple" href="https://developers.ripple.com/build-run-rippled-ubuntu.html" target="_blank"
                  style="text-decoration:none;">RIPPLED</a> version <span style="color: #400080;" v-if="serverState !== null">
                  {{ serverState.build_version }}</span></div>
            </div>
          </div>
        </div>
      </div>

      <div class="margtop">
        <div class="container-fluid" align="center">
          <br />
          <div id="accordian">
            <div class="card" style="width: 885px;">
              <div class="card-header" id="headingOne" align="left">
                <h5 class="mb-0">
                  <button class="btn btn-link collapsed" data-toggle="collapse" data-target="#collapseOne"
                    aria-expanded="false" aria-controls="collapseOne">
                    Validation Metrics - Short </button>
                  <button class="btn btn-link collapsed float-right" data-toggle="collapse" data-target="#collapseOne"
                    aria-expanded="false" aria-controls="collapseOne">
                    <div class="picright"> <img src="../assets/xrpsm.png" /></div>
                  </button>
                </h5>
              </div>
              <div id="collapseOne" class="collapse" aria-labelledby="headingOne" data-parent="#accordian">
                <div class="card-body">
                  <div class="row">
                    <div class="serverst d-none d-sm-block col-sm-3 text-right">Server State</div>
                    <div class="serverst d-block d-sm-none col-12 text-left text-muted">Server State</div>
                    <div class="serverrt col-12 col-sm-9"><b v-if="serverState !== null"> <span class="text-success">{{
                          serverState.server_state }}</span></b></div>

                    <div class="serverst d-none d-sm-block col-sm-3 text-right">Build version</div>
                    <div class="serverst d-block d-sm-none col-12 text-left text-muted">Build version</div>
                    <div class="serverrt col-12 col-sm-9"><span v-if="serverState !== null"> {{
                        serverState.build_version }}</span></div>

                    <div class="serverst d-none d-sm-block col-sm-3 text-right">Server Uptime</div>
                    <div class="serverst d-block d-sm-none col-12 text-left text-muted">Server Uptime</div>
                    <div class="serverrt col-12 col-sm-9"><span v-if="serverState !== null">{{
                        Math.round(serverState.uptime/86400 * 100)/ 100}} days</span></div>


                    <div class="serverst d-none d-sm-block col-sm-3 text-right">Peers</div>
                    <div class="serverst d-block d-sm-none col-12 text-left text-muted">Peers</div>
                    <div class="serverrt col-12 col-sm-9"><span v-if="serverState !== null"> {{ serverState.peers }}</span></div>


                    <div class="serverst d-none d-sm-block col-sm-3 text-right">Last ledger</div>
                    <div class="serverst d-block d-sm-none col-12 text-left text-muted">Last ledger</div>
                    <div class="serverrt col-12 col-sm-9"><b v-if="serverState !== null"> {{
                        serverState.validated_ledger.seq }}</b></div>


                    <div class="serverst d-none d-sm-block col-sm-3 text-right">Complete Ledgers</div>
                    <div class="serverst d-block d-sm-none col-12 text-left text-muted">Complete Ledgers</div>
                    <div class="serverrt col-12 col-sm-9"><span v-if="serverState !== null"> {{
                        serverState.complete_ledgers }}</span></div>


                    <div class="serverst d-none d-sm-block col-sm-3 text-right">Last Ledger #</div>
                    <div class="serverst d-block d-sm-none col-12 text-left text-muted">Last Ledger #</div>
                    <div class="serverrt col-12 col-sm-9"><span v-if="serverState !== null"> {{
                        serverState.validated_ledger.hash }}</span></div>


                    <div class="serverst d-none d-sm-block col-sm-3 text-right">Public Key</div>
                    <div class="serverst d-block d-sm-none col-12 text-left text-muted">Public Key</div>
                    <div class="serverrt col-12 col-sm-9"><span v-if="serverState !== null"> {{ serverState.pubkey_node
                        }}</span></div>
                  </div>
                </div>
              </div>


              <div class="card" style="width: 885px;">
              </div>

              <div class="card-header" id="headingTwo" align="left">
                <h5 class="mb-0">
                  <button class="btn btn-link collapsed" data-toggle="collapse" data-target="#collapseTwo"
                    aria-expanded="false" aria-controls="collapseTwo">
                    Validation Metrics - Raw Data
                  </button>
                  <button class="btn btn-link collapsed float-right" data-toggle="collapse" data-target="#collapseTwo"
                    aria-expanded="false" aria-controls="collapseTwo">
                    <div class="picright"> <img src="../assets/xrpsm.png" /></div>
                  </button>

                </h5>
              </div>


              <div id="collapseTwo" class="collapse" aria-labelledby="headingTwo" data-parent="#accordian">
                <div class="card-body">
                  <div class="row">

                    <div style="font-size: 1.1em; text-align: left;" v-if="serverState !== null">
                      <pre>{{ serverState }}</pre>
                      <br /><br />
                    </div>
                  </div>
                </div>
              </div>

              <div class="card" style="width: 885px;">
              </div>

              <div class="card-header" id="headingThree" align="left">
                <h5 class="mb-0">
                  <button class="btn btn-link collapsed" data-toggle="collapse" data-target="#collapseThree"
                    aria-expanded="false" aria-controls="collapseThree">
                    Validation History
                  </button>
                  <button class="btn btn-link collapsed float-right" data-toggle="collapse" data-target="#collapseThree"
                    aria-expanded="false" aria-controls="collapseThree">
                    <div class="picright"> <img src="../assets/xrpsm.png" /></div>
                  </button>
                </h5>
              </div>
              <div id="collapseThree" class="collapse" aria-labelledby="headingThree" data-parent="#accordian">
                <div class="card-body">
                  <div class="row">
                    <div v-if="!validatorData.reports">
                      Loading...
                    </div>
                    <div v-else>
                      <div class="table-responsive" style="margin-left: -6px; margin-top: -21px; width: 885px;">
                        <table class="table table-striped">
                          <thead>
                            <tr class="colorrow" style="width: 885px;">
                              <th class="text-left" scope="col">&nbsp</th>
                              <th class="text-left" scope="col">Date</th>
                              <th class="text-left" scope="col">Agreement</th>
                              <th class="text-left" scope="col">Disagreement</th>
                              <th class="text-left" scope="col">Total Validations</th>
                            </tr>
                          </thead>
                          <tbody>
                            <tr scope="row" v-for="r in validatorData.reports.slice().reverse()" v-bind:key="r.date">
                              <td></td>
                              <td>{{ new Date(Date.parse(r.date)).toLocaleDateString('en-NZ',{ weekday: 'long', month:
                                'long', day: 'numeric', year: 'numeric'}).replace(',',' ') }}</td>
                              <td>{{ r.main_net_agreement }}</td>
                              <td>{{ (((r.total_ledgers - r.main_net_ledgers) / r.total_ledgers) .toFixed(5)) }} </td>
                              <td>{{ r.main_net_ledgers }}</td>
                            </tr>
                          </tbody>
                        </table>
                      </div>
                    </div>
                  </div>
                </div>
              </div>


              <div class="card" style="width: 885px;">
              </div>

              <div class="card-header" id="headingFour" align="left">
                <h5 class="mb-0">
                  <button class="btn btn-link collapsed" data-toggle="collapse" data-target="#collapseFour"
                    aria-expanded="false" aria-controls="collapseFour">
                    Validators - ALL
                  </button>
                  <button class="btn btn-link collapsed float-right" data-toggle="collapse" data-target="#collapseFour"
                    aria-expanded="false" aria-controls="collapseFour">
                    <div class="picright"> <img src="../assets/xrpsm.png" /></div>
                  </button>
                </h5>
              </div>
              <div id="collapseFour" class="collapse" aria-labelledby="headingFour" data-parent="#accordian">
                <div class="card-body">




                  <div class="row">
                    <div v-if="!validatorsData.reports">
                      Loading...
                    </div>
                    <div v-else>
                      <div class="table-responsive" style="margin-left: -6px; margin-top: -21px; width: 885px;">
                        <table class="table table-striped">
                          <thead>
                            <tr class="colorrow" style="width: 885px;">
                              <th class="text-left" scope="col">Validation Public Key</th>
                              <th class="text-left" scope="col">Verification Status</th>
                              <th class="text-left" scope="col">Domain Name</th>

                            </tr>
                          </thead>
                          <tbody>
                            <tr scope="row" v-for="q in validatorsData.reports" v-bind:key="q.main_net_ledgers">
                              <td class="Tab4"><a v-bind:href="'https://xrpcharts.ripple.com/#/validators/' + q.validation_public_key"
                                  target="_blank" style="text-decoration:none;"> {{ q.validation_public_key }}</a></td>
                              <td class="Tab4">
                                <p style="text-align: center; color: darkgreen; margin-bottom: 0; font-size: 20px;"><template
                                    v-if="q.domain_state"> &#10004 </template></p>
                              </td>
                              <td class="Tab4">
                                <p style="margin-bottom: 0;" v-if="q.domain_state"></p>
                                <p style="margin-bottom: 0; color: darkblue; font-style: italic;" v-else> not yet
                                  verified</p> <a v-bind:href="q.domain" target="_blank" style="text-decoration:none;">{{
                                  q.domain }}</a>
                              </td>
                            </tr>
                          </tbody>
                        </table>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="card" style="width: 885px;">
              </div>

              <div class="card-header" id="headingFive" align="left">
                <h5 class="mb-0">
                  <button class="btn btn-link collapsed" data-toggle="collapse" data-target="#collapseFive"
                    aria-expanded="false" aria-controls="collapseFive">
                    Daily XRP Exchange Sales
                  </button>
                  <button class="btn btn-link collapsed float-right" data-toggle="collapse" data-target="#collapseFive"
                    aria-expanded="false" aria-controls="collapseFive">
                    <div class="picright"> <img src="../assets/xrpsm.png" /></div>
                  </button>
                </h5>
              </div>
              <div id="collapseFive" class="collapse" aria-labelledby="headingFive" data-parent="#accordian">
                <div class="card-body">
                  <div class="row">
                    <div v-if="!xrpData">
                      Loading...
                    </div>
                    <div v-else>
                      <div class="table-responsive" style="margin-left: -6px; margin-top: -20px; width: 885px;">

                        <table class="table table-striped">
                          <thead>
                            <tr class="colorrow2" style="width: 885px;">
                              <th>&nbsp&nbsp</th>
                              <th class="text-left" scope="col">Date</th>
                              <th>&nbsp&nbsp</th>
                              <th class="text-left" scope="col">Period</th>
                              <th class="text-left">&nbsp&nbsp</th>
                              <th class="text-left" scope="col">Total Volume of XRP</th>
                            </tr>
                          </thead>
                          <tbody>
                            <tr style="color: darkblue;" scope="row" v-for="w in xrpData" v-bind:key="w.date">
                              <td />
                              <td>{{ new Date(Date.parse(w.date)).toLocaleDateString('en-NZ',{ weekday: 'long', month:
                                'long', day: 'numeric', year: 'numeric', hour: '2-digit', minute: '2-digit', second:
                                '2-digit', timeZoneName: 'short'}).replace(',',' ') }}</td>
                              <td />
                              <td>{{ w .period }}</td>
                              <td />
                              <td>{{ w.total }} xrp</td>
                            </tr>
                          </tbody>
                        </table>
                      </div>

                      <div class="table-responsive" style="margin-left: -6px; margin-top: -15px; width: 885px;">
                        <table class="table table-striped">
                          <thead>
                            <tr class="colorrow" style="width: 885px;">
                              <th class="text-left" scope="col">Exchange</th>
                              <th class="text-left" scope="col">Volume</th>
                              <th class="text-left" scope="col">Trades</th>
                              <th class="text-left" scope="col">Avg Trade</th>
                              <th class="text-left" scope="col">Against</th>
                            </tr>
                          </thead>
                          <tbody>
                            <tr scope="row" v-for="w in xrpData.data.components.slice().sort()" v-bind:key="w.base_volume">

                              <td>{{ w.source }} </td>
                              <td>{{ (w.base_volume * 1).toFixed(5) }}</td>
                              <td>{{ w.count }}</td>
                              <td>{{ (w.base_volume / w.count).toFixed(5) }}</td>
                              <td>{{ w.counter_currency }}</td>



                            </tr>
                          </tbody>
                        </table>
                      </div>
                    </div>
                  </div>
                </div>
              </div>




            </div>
          </div>
        </div>
      </div>
      <div>
        <p>&nbsp</p>
        <p>&nbsp</p>
        <p>&nbsp</p>
      </div>

      <div class="w-100">
        <div class="footer" pt-5>
          <div class="row">
            &#169; rippleitin.nz 2018 - new zealand's original XRP validating node
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
    data() {
      return {
        cryptos: {
          XRP: {}
        },
        serverState: null,
        //Rippled Lookup my validator
        appName: "SampleApp",
        validatorData: {},
        //Rippled end

        //Rippled Lookup all validators
        appName: "ValApp",
        validatorsData: {},
        //Rippled end

        //Rippled Lookup all xrp sales
        appName: "XRPSales",
        xrpData: {}

        //Rippled end
      };
    },
    mounted() {
      // Run immediately
      this.fetchRates();
      // Set interval at 20 seconds (re-run)
      apiInterval = setInterval(this.fetchRates, 20 * 1000);
      // Connect to server status
      this.serverStateHandler();

      //rippled Lookup my validator
      window
        .fetch(
          "https://data.ripple.com/v2/network/validators/nHUcQnmEbCNq4uhntFudzfrZV8P5WLoBrR5h3R9jAd621Aaz1pSy/reports"
        )
        .then(r => {
          return r.json();
        })
        .then(data => {
          // Set it to the Vue App data
          this.validatorData = data;
        });
      //rippled lookup end

      //rippled Lookup all validators
      window
        .fetch("https://data.ripple.com/v2/network/validator_reports")
        .then(q => {
          return q.json();
        })
        .then(data => {
          // Set it to the Vue App data
          this.validatorsData = data;
        });
      //rippled lookup xrp sales

      window
        .fetch("https://data.ripple.com/v2/network/external_markets")
        .then(w => {
          return w.json();
        })
        .then(data => {
          // Set it to the Vue App data
          this.xrpData = data;
        });
      //rippled lookup end
    },
    destroyed() {
      clearInterval(apiInterval);
    },

    methods: {
      serverStateHandler() {
        const s = new WebSocket("wss://rippleitin.nz");
        s.onmessage = m => {
          this.serverState = JSON.parse(m.data);
        };
      },
      fetchRates() {
        axios
          .get(
            "https://min-api.cryptocompare.com/data/pricemulti?fsyms=XRP&tsyms=USD,EUR,GBP,AUD,NZD"
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
  };

</script>

<style lang="scss">
  #app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin-top: -4px;
    align: center;
  }

  // buttons

  .Tab4 {
    color: black;
    font-size: 13px;
    padding: 0 0 0 0;
  }

  .card-header {
    padding: 0.5rem 0.3re 0 0.3rem;
  }

  .darkgreen {
    color: darkgreen;
    display: flex;
    justify-content: left;
    align-items: middle;
    margin-left: 30px;
    align: center;
  }

  .card .btn-link {
    color: darkgreen;
  }

  .card .btn-link:hover {
    color: #336699;
  }

  .nav-link:hover {
    color: #400080;
  }

  .nav-link {
    color: #7300e6;
  }

  .picright {
    float: right;
    margin-top: -5px;
    margin-bottom: -5px;
  }

  .table-striped>tbody>tr:nth-child(odd)>td,
  .table-striped>tbody>tr:nth-child(odd)>th {
    background-color: #f4fef4;
  }

  .colorrow {
    width: 100%;
    background-image: linear-gradient(90deg, #84fab0 0%, #8fd3f4 100%);
    background-attachment: fixed;
    border-top: solid 1px #000;
    border-bottom: solid 1px #000;
  }

  .colorrow2 {
    width: 100%;
    background-image: linear-gradient(90deg, #84fab0 0%, #8fd3f4 100%);
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
    color: #400080;
  }

  .ripple {
    color: #7300e6;
  }

  .jumbotron {
    background-image: linear-gradient(120deg, #8fd3f4 0%, #84fab0 100%);
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
    margin-top: -10px;
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
    margin-left: 0;
    margin-right: 0;
    display: flex;
    justify-content: center;
    align-items: middle;
    margin-top: 16px;
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

  .console-container {
    font-size: 16px;
    align: left;
    color: #000;
    margin-top: -4px;
  }

  .serverstate {
    font-size: 14px;
  }

  .rate {
    margin-left: 10px;
  }

  .serverst {
    color: green;
    padding: 2px 0 3.75px 0px;
    margin-left: -35px;
  }

  .serverrt {
    color: #2c3e50;
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

  .console-underscore {
    display: inline-block;
    position: relative;
    left: 1px;
    color: #000;
    top: 0.05em;
  }

  .hidden {
    opacity: 0;
  }

  .footer {
    height: 40px;
    background-image: linear-gradient(120deg, #84fab0 0%, #8fd3f4 100%);
    font-size: 13px;
    bottom: 0;
    width: 100%;
    color: #000;
    position: fixed;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 4%;
    border-top: solid black 0.75px;
    border-bottom: solid black 0.75px;
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
  .btn-primary.focus {}

  .btn-primary.disabled,
  .btn-primary:disabled {
    color: #7300e6;
    background-color: #7cc;
    border-color: #400080;
  }

  .btn-primary:not(:disabled):not(.disabled):active,
  .btn-primary:not(:disabled):not(.disabled).active,
  .show>.btn-primary.dropdown-toggle {
    color: #7300e6;
    background-color: #9cdada;
    border-color: #2e7c7c;
  }

  .btn-primary:not(:disabled):not(.disabled):active:focus,
  .btn-primary:not(:disabled):not(.disabled).active:focus,
  .show>.btn-primary.dropdown-toggle:focus {}

  .btn-outline-primary {
    color: #7300e6;
    background-color: transparent;
    background-image: none;
    border-color: #7300e6;
  }

  .btn-outline-primary:hover {
    color: #400080;
    background-image: linear-gradient(120deg, #8fd3f4 0%, #84fab0 100%);
    border-color: #400080;
  }

  .btn-outline-primary:focus,
  .btn-outline-primary.focus {}

  .btn-outline-primary.disabled,
  .btn-outline-primary:disabled {
    color: #7cc;
    background-color: transparent;
  }

  .btn-outline-primary:not(:disabled):not(.disabled):active,
  .btn-outline-primary:not(:disabled):not(.disabled).active,
  .show>.btn-outline-primary.dropdown-toggle {
    color: #7300e6;
    background-color: #8ad3d3;
    border-color: #7cc;
  }

  .btn-outline-primary:not(:disabled):not(.disabled):active:focus,
  .btn-outline-primary:not(:disabled):not(.disabled).active:focus,
  .show>.btn-outline-primary.dropdown-toggle:focus {}

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

</style>
