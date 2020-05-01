<template>
  <div
    class="cardouter tab-pane h-100 p-3 rounded"
    id="rippleitinnz"
    role="tabpanel"
    aria-labelledby="rippleitinnz-tab"
  >
    <div class="card" style="width: 885px;margin: 15px;">
      <div id="accordian2">
        <div class="card-header" id="headingFive" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapseFive"
              aria-expanded="false"
              aria-controls="collapseFive"
            >Rippleitin Validator Metrics - Short</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapseFive"
              aria-expanded="false"
              aria-controls="collapseFive"
            >
              <div class="picright">
                <img class="xrpJPG" />
              </div>
            </button>
          </h5>
        </div>

        <div
          id="collapseFive"
          class="collapse"
          aria-labelledby="headingFive"
          data-parent="#accordian2"
        >
          <div class="card-body">
            <div class="row" style="font-size: 0.87rem;">
              <div class="serverst d-none d-sm-block col-sm-3 text-right">Server State</div>
              <div class="serverst d-block d-sm-none col-12 text-left text-muted">Server State</div>
              <div class="serverrt col-12 col-sm-9">
                <b v-if="serverStates !== null">
                  <span class="text-success">{{ serverStates.server_state }}</span>
                </b>
              </div>

              <div class="serverst d-none d-sm-block col-sm-3 text-right">IO Latency</div>
              <div class="serverst d-block d-sm-none col-12 text-left text-muted">IO Latency</div>
              <div class="serverrt col-12 col-sm-9">
                <span v-if="serverStates !== null">{{ serverStates.io_latency_ms}}ms</span>
              </div>

              <div class="serverst d-none d-sm-block col-sm-3 text-right">Build version</div>
              <div class="serverst d-block d-sm-none col-12 text-left text-muted">Build version</div>
              <div class="serverrt col-12 col-sm-9">
                <span v-if="serverStates !== null">{{ serverStates.build_version }}</span>
              </div>

              <div class="serverst d-none d-sm-block col-sm-3 text-right">Server Uptime</div>
              <div class="serverst d-block d-sm-none col-12 text-left text-muted">Server Uptime</div>
              <div class="serverrt col-12 col-sm-9">
                <span
                  v-if="serverStates !== null"
                >{{ Math.round(serverStates.uptime/86400 * 100)/ 100}} days</span>
              </div>

              <div class="serverst d-none d-sm-block col-sm-3 text-right">Peers</div>
              <div class="serverst d-block d-sm-none col-12 text-left text-muted">Peers</div>
              <div class="serverrt col-12 col-sm-9">
                <span v-if="serverStates !== null">{{ serverStates.peers }}</span>
              </div>

              <div
                class="serverst1 d-none d-sm-block col-sm-3 text-right"
                style="font-size: 0.87rem;"
              >Last Closed Ledger</div>
              <div
                class="serverst1 d-block d-sm-none col-12 text-left text-muted"
                style="font-size: 0.87rem;"
              >Last Closed Ledger</div>
              <div class="serverrt1 col-12 col-sm-9">
                <b v-if="serverStates !== null">{{ serverStates.validated_ledger.seq }}</b>
              </div>

              <div class="serverst d-none d-sm-block col-sm-3 text-right">Complete Ledgers</div>
              <div class="serverst d-block d-sm-none col-12 text-left text-muted">Complete Ledgers</div>
              <div class="serverrt col-12 col-sm-9">
                <span v-if="serverStates !== null">{{ serverStates.complete_ledgers }}</span>
              </div>

              <div class="serverst d-none d-sm-block col-sm-3 text-right">Last Ledger #</div>
              <div class="serverst d-block d-sm-none col-12 text-left text-muted">Last Ledger #</div>
              <div class="serverrt col-12 col-sm-9">
                <span v-if="serverStates !== null">{{ serverStates.validated_ledger.hash }}</span>
              </div>

              <div class="serverst d-none d-sm-block col-sm-3 text-right">Public Key</div>
              <div class="serverst d-block d-sm-none col-12 text-left text-muted">Public Key</div>
              <div class="serverrt col-12 col-sm-9">
                <span>nHUcQnmEbCNq4uhntFudzfrZV8P5WLoBrR5h3R9jAd621Aaz1pSy</span>
              </div>
            </div>
          </div>
        </div>
        <!-- Card Six-->
        <div class="card-header" id="headingSix" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapseSix"
              aria-expanded="false"
              aria-controls="collapseSix"
            >Rippleitin Validator Metrics - Raw Data</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapseSix"
              aria-expanded="false"
              aria-controls="collapseSix"
            >
              <div class="picright">
                <img class="xrpJPG" />
              </div>
            </button>
          </h5>
        </div>

        <div
          id="collapseSix"
          class="collapse"
          aria-labelledby="headingSix"
          data-parent="#accordian2"
        >
          <div class="card-body">
            <div class="row">
              <div style="font-size: 1.0em; text-align: left;" v-if="serverStates !== null">
                <pre class="preClass">{{ serverStates }}</pre>
                <br />
                <br />
              </div>
            </div>
          </div>
        </div>

        <div class="card-header" id="headingSeven" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapseSeven"
              aria-expanded="false"
              aria-controls="collapseSeven"
            >Rippleitin Validator Hardware</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapseSeven"
              aria-expanded="false"
              aria-controls="collapseSeven"
            >
              <div class="picright">
                <img class="xrpJPG" />
              </div>
            </button>
          </h5>
        </div>
        <!-- Card 7-->
        <div
          id="collapseSeven"
          class="collapse"
          aria-labelledby="headingSeven"
          data-parent="#accordian2"
        >
          <div class="card-body">
            <div class="row">
              <div class="serverst d-none d-sm-block col-sm-3 text-right">Motherboard</div>
              <div class="serverst d-block d-sm-none col-12 text-left text-muted">Motherboard</div>
              <div class="serverrt col-12 col-sm-9">
                <span>ASUS Prime B350M-A Socket AM4</span>
              </div>

              <div class="serverst d-none d-sm-block col-sm-3 text-right">CPU</div>
              <div class="serverst d-block d-sm-none col-12 text-left text-muted">CPU</div>
              <div class="serverrt col-12 col-sm-9">
                <span>AMD Ryzen 2200G Socket AM4 4 Core 4 Threads 3.5GHz</span>
              </div>

              <div class="serverst d-none d-sm-block col-sm-3 text-right">RAM</div>
              <div class="serverst d-block d-sm-none col-12 text-left text-muted">RAM</div>
              <div class="serverrt col-12 col-sm-9">
                <span>32GB Crucial DDR4 2400 PC4-19200 CL17</span>
              </div>

              <div class="serverst d-none d-sm-block col-sm-3 text-right">SSD</div>
              <div class="serverst d-block d-sm-none col-12 text-left text-muted">SSD</div>
              <div class="serverrt col-12 col-sm-9">
                <span>Samsung 970 EVO 250GB M.2 NVMe SSD R/W 3400MB/s 1500MB/s 200k/350k IOPS</span>
              </div>

              <div class="serverst d-none d-sm-block col-sm-3 text-right">OS</div>
              <div class="serverst d-block d-sm-none col-12 text-left text-muted">OS</div>
              <div class="serverrt col-12 col-sm-9">
                <span>CentOS 7</span>
              </div>

              <div class="serverst d-none d-sm-block col-sm-3 text-right">Server Location</div>
              <div class="serverst d-block d-sm-none col-12 text-left text-muted">Server Location</div>
              <div class="serverrt col-12 col-sm-9">
                <span>New Zealand</span>
              </div>

              <div class="serverst d-none d-sm-block col-sm-3 text-right">Internet Connectivity</div>
              <div
                class="serverst d-block d-sm-none col-12 text-left text-muted"
              >Internet Connectivity</div>
              <div class="serverrt col-12 col-sm-9">
                <span>Gigabit Fibre with 4G Cellular redundancy</span>
              </div>

              <div class="serverst d-none d-sm-block col-sm-3 text-right">Server Admin Contact</div>
              <div
                class="serverst d-block d-sm-none col-12 text-left text-muted"
              >Server Admin Contact</div>
              <div class="serverrt col-12 col-sm-9">
                <span>
                  <a href="mailto:admin@rippleitin.nz">admin@rippleitin.nz</a>
                </span>
              </div>
            </div>
          </div>
        </div>

        <!-- card eight-->
        <div class="card-header" id="headingEight" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapseEight"
              aria-expanded="false"
              aria-controls="collapseEight"
            >Rippleitin Validation History</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapseEight"
              aria-expanded="false"
              aria-controls="collapseEight"
            >
              <div class="picright">
                <img class="xrpJPG" />
              </div>
            </button>
          </h5>
        </div>
        <div
          id="collapseEight"
          class="collapse"
          aria-labelledby="headingEight"
          data-parent="#accordian2"
        >
          <div class="card-body">
            <div class="row">
              <div v-if="!validatorData.reports">Loading...</div>
              <div v-else>
                <div
                  class="table-responsive"
                  style="margin-left: -6px; margin-top: -20px; width: 885px;"
                >
                  <table class="table table-striped">
                    <thead>
                      <tr class="colorrow2" style="width: 885px;">
                        <th class="text-left" scope="col">&nbsp;</th>
                        <th class="text-left" scope="col">Date</th>
                        <th class="text-right" scope="col">24hr Total</th>
                        <th class="text-right" scope="col">Missed</th>
                        <th class="text-right" scope="col"></th>
                        <th class="text-center" scope="col">Score</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr
                        scope="row"
                        v-for="r in validatorData.reports.slice().reverse()"
                        v-bind:key="`idb-${r.date}`"
                      >
                        <td></td>
                        <td>{{ new Date(Date.parse(r.date)).toLocaleDateString('en-NZ',{ weekday: 'long', month: 'long', day: 'numeric', year: 'numeric'}).replace(',',' ') }}</td>
                        <td class="text-right">{{ r.total }}</td>
                        <td class="text-right">
                          <p style="color: red; margin-bottom: 0;" v-if="r.missed>0">{{ r.missed }}</p>
                        </td>
                        <td class="text-center"></td>
                        <td class="text-center">
                          <p
                            style="margin-bottom: 0;"
                            v-if="r.score<1"
                          >{{ (r.score * 100).toFixed(2) }}%</p>
                          <p style="margin-bottom: 0;" v-else>100%</p>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- card vote-->

        <div class="card-header" id="heading12" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapse12"
              aria-expanded="false"
              aria-controls="collapse12"
            >Rippleitin Vote History</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapse12"
              aria-expanded="false"
              aria-controls="collapse12"
            >
              <div class="picright">
                <img class="xrpJPG" />
              </div>
            </button>
          </h5>
        </div>

        <div id="collapse12" class="collapse" aria-labelledby="heading12" data-parent="#accordian2">
          <div class="card-body">
            <div class="row">
              <div
                class="table-responsive"
                style="margin-left: -6px; margin-right: -6px;margin-top: -20px; width: 885px;"
              >
                <table class="table table-striped">
                  <thead>
                    <tr class="colorrow2" style="width: 885px;">
                      <th class="text-left" scope="col">&nbsp;</th>
                      <th class="text-left" scope="col">Name</th>
                      <th class="text-left" scope="col">XRPL Amendment Status</th>
                      <th class="text-left" scope="col">&nbsp;</th>
                      <th class="text-left" scope="col">Current Count/Threshold</th>
                      <th class="text-left" scope="col">&nbsp;</th>
                      <th class="text-left" scope="col">Our Vote</th>

                      <th class="text-left" scope="col">&nbsp;</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="bash in Object.keys(featureStates)" v-bind:key="`idc-${bash}`">
                      <td class="text-left" scope="col">&nbsp;</td>
                      <td
                        class="text-left"
                        :class="{ 'orange': featureStates[bash].count}"
                      >{{ featureStates[bash].name }}</td>
                      <td
                        class="text-left"
                        :class="{ 'orange': featureStates[bash].count}"
                      >{{ featureStates[bash].enabled ? 'Enabled' : 'Disabled' }}</td>
                      <td class="text-left" scope="col">&nbsp;</td>

                      <td class="text-left" :class="{ 'blue': featureStates[bash].count }">
                        <div v-if="!featureStates[bash].count"></div>
                        <div
                          v-else
                        >{{ featureStates[bash].count }}/{{ featureStates[bash].threshold }}</div>
                      </td>

                      <td class="text-left" scope="col">&nbsp;</td>
                      <td
                        class="text-left"
                        :class="{ 'red': featureStates[bash].vetoed }"
                      >{{ featureStates[bash].vetoed ? 'Vetoed' : 'Supported' }}</td>

                      <td class="text-left" scope="col">&nbsp;</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>

        <!--vote -->
      </div>
    </div>
  </div>

  <!--Tab 3 No Cards-->
</template>

<script>
//let sockInterval;

export default {
  data: function() {
    return {
      //cryptoprices
      cryptos: {
        XRP: {}
      },

      props: {
        isTrue: Boolean
      },

      //cryptoprices
      scryptos: {
        sXRP: {}
      },

      //Rawdata from my validator
      serverStates: null, //rippleitinnz server
      featureStates: null, //rippleitinz voting
      //  serverWebsock: null, //s1ripple

      //Rippled Lookup my validator
      appName: "MyVal",
      validatorData: {}
    };
  },
  //end of data

  mounted() {
    // Connect to server status
    this.serverStateHand();
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
  },

  //  destroyed() {
  //    clearInterval(sockInterval);
  // },
  methods: {
    serverStateHand() {
      const endpoint =
        process.env.NODE_ENV === "development"
          ? "ws://192.168.1.50:8181"
          : "wss://rippleitin.nz";
      const sk = new WebSocket(endpoint);
      sk.onmessage = mk => {
        const parsedJson = JSON.parse(mk.data);
        if (typeof parsedJson.build_version === "undefined") {
          this.featureStates = parsedJson;
          console.log(parsedJson);
        } else {
          this.serverStates = parsedJson;
        }
      };
    }
  }
  //end of methods
};
</script>

