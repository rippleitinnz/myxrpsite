<template>
  <!-- Tab One Card One-->
  <div
    class="tab-pane h-100 p-3 border border-info rounded"
    id="XRPL"
    role="tabpanel"
    aria-labelledby="XRPL-tab"
  >
    <div class="card" style="width: 885px; margin-top: 25px;">
      <div id="accordian">
        <!--Card One -->
        <div class="card-header" id="headingOne" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapseOne"
              aria-expanded="false"
              aria-controls="collapseOne"
            >XRP Ledger Metrics</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapseOne"
              aria-expanded="false"
              aria-controls="collapseOne"
            >
              <div class="picright">
                <img src="../assets/xrpsm.png">
              </div>
            </button>
          </h5>
        </div>

        <div
          id="collapseOne"
          class="collapse"
          aria-labelledby="headingOne"
          data-parent="#accordian"
        >
          <div class="card-body">
            <div class="row">
              <div v-if="!xrpMetrics.stats">Loading...</div>
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
                        <th class="text-right" scope="col">Ledger Count</th>
                        <th class="text-right" scope="col">Ledger Interval</th>
                        <th class="text-right" scope="col">Total Transactions</th>
                        <th class="text-center" scope="col">TX per Ledger</th>
                        <th class="text-center" scope="col">New Accounts</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr scope="row" v-for="p in xrpMetrics.stats" v-bind:key="p.date">
                        <td></td>
                        <td
                          class="text-left"
                        >{{ new Date(Date.parse(p.date)).toLocaleDateString('en-NZ',{ month: 'long', day: 'numeric', year: 'numeric'}).replace(',',' ') }}</td>
                        <td class="text-right">{{ p.metric.ledger_count }}</td>
                        <td class="text-right">{{ (p.metric.ledger_interval * 1).toFixed(5)}}</td>
                        <td class="text-right">{{ p.metric.transaction_count }}</td>
                        <td class="text-right">{{ (p.metric.tx_per_ledger * 1).toFixed(5)}}</td>
                        <td class="text-right">{{ p.metric.accounts_created }} &nbsp; &nbsp;</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!--Card OneA -->
        <div class="card-header" id="headingOne2" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapseOne2"
              aria-expanded="false"
              aria-controls="collapseOne2"
            >XRP Ledger Fees</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapseOne2"
              aria-expanded="false"
              aria-controls="collapseOne2"
            >
              <div class="picright">
                <img src="../assets/xrpsm.png">
              </div>
            </button>
          </h5>
        </div>

        <div
          id="collapseOne2"
          class="collapse"
          aria-labelledby="headingOne2"
          data-parent="#accordian"
        >
          <div class="card-body">
            <div class="row">
              <div v-if="!xrpBurn.rows">Loading...</div>
              <div v-else>
                <div
                  class="table-responsive"
                  style="margin-left: -6px; margin-top: -20px; width: 885px;"
                >
                  <table class="table table-striped">
                    <thead>
                      <tr class="colorrow2" style="width: 885px;">
                        <th class="text-left" scope="col">Date</th>
                        <th class="text-left" scope="col"></th>
                        <th class="text-center" scope="col">Total XRP Burned to Date</th>
                        <th class="text-left" scope="col"></th>
                        <th class="text-center" scope="col">Total XRP left in existence</th>
                        <th class="text-left" scope="col"></th>
                        <th class="text-center" scope="col">% of original XRP burnt</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr
                        scope="row"
                        v-for="k in xrpBurn.rows.slice().reverse()"
                        v-bind:key="k.date"
                      >
                        <td>{{ new Date(Date.parse(k.date)).toLocaleDateString('en-NZ',{ weekday: 'long', month: 'long', day: 'numeric', year: 'numeric'}).replace(',',' ') }}</td>
                        <td></td>
                        <td class="text-center">{{ 100000000000 - k.total }}</td>
                        <td></td>
                        <td class="text-center">{{ k.total }}</td>
                        <td></td>
                        <td class="text-center">{{ (100000000000 - k.total) / 100000000000 }}</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>

            <div class="row">
              <div v-if="!xrpFees.rows">Loading...</div>
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
                        <th class="text-right" scope="col">Total Transactions</th>
                        <th class="text-right" scope="col">Minimum Fee</th>
                        <th class="text-right" scope="col">Maximum Fee</th>
                        <th class="text-center" scope="col">Average Fee</th>
                        <th class="text-center" scope="col">Total Fees (Burn)</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr scope="row" v-for="d in xrpFees.rows" v-bind:key="d.date">
                        <td></td>
                        <td
                          class="text-left"
                        >{{ new Date(Date.parse(d.date)).toLocaleDateString('en-NZ',{ month: 'long', day: 'numeric', year: 'numeric'}).replace(',',' ') }}</td>
                        <td class="text-right">{{ d.tx_count }}</td>
                        <td class="text-right">{{ d.min }}</td>
                        <td class="text-right">{{ d.max }}</td>
                        <td class="text-right">{{ d.avg }}</td>
                        <td class="text-right">{{ d.total}} &nbsp; &nbsp;</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- card two-->
        <div class="card-header" id="headingTwo" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapseTwo"
              aria-expanded="false"
              aria-controls="collapseTwo"
            >XRP Transactions - Previous 24hrs - by Exchange</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapseTwo"
              aria-expanded="false"
              aria-controls="collapseTwo"
            >
              <div class="picright">
                <img src="../assets/xrpsm.png">
              </div>
            </button>
          </h5>
        </div>
        <div
          id="collapseTwo"
          class="collapse"
          aria-labelledby="headingTwo"
          data-parent="#accordian"
        >
          <div class="card-body">
            <div class="row">
              <div v-if="!xrpData.data">Loading...</div>
              <div v-else>
                <div
                  class="table-responsive"
                  style="margin-left: -6px; margin-top: -20px; width: 885px;"
                >
                  <table class="table table-striped">
                    <thead>
                      <tr class="colorrow2" style="width: 885px;">
                        <th class="text-left" scope="col">&nbsp;&nbsp;</th>
                        <th class="text-left" scope="col">Date</th>
                        <th class="text-left" scope="col">&nbsp;&nbsp;</th>
                        <th class="text-left" scope="col">Period</th>
                        <th class="text-left" scope="col">&nbsp;&nbsp;</th>
                        <th class="text-left" scope="col">Total Volume of XRP</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr style="color: darkblue;" scope="row">
                        <td/>
                        <td>
                          {{ new Date(Date.parse(xrpData.data.date)).toLocaleDateString('en-NZ',{ weekday: 'long', month:
                          'long', day: 'numeric', year: 'numeric', hour: '2-digit', minute: '2-digit', second: '2-digit', timeZoneName: 'short'}).replace(',',' ') }}
                        </td>
                        <td/>
                        <td>Last 24 hours</td>
                        <td/>
                        <td>{{ (xrpData.data.total* 1).toFixed(0) }} xrp</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
                <div
                  class="table-responsive"
                  style="margin-left: -6px; margin-top: -20px; width: 885px;"
                >
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
                      <tr
                        scope="row"
                        v-for="w in xrpData.data.components.slice().sort()"
                        v-bind:key="w.base_volume"
                      >
                        <td>{{ w.source }}</td>
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

        <!-- Card three-->
        <div class="card-header" id="headingThree" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapseThree"
              aria-expanded="false"
              aria-controls="collapseThree"
            >XRP Distribution</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapseThree"
              aria-expanded="false"
              aria-controls="collapseThree"
            >
              <div class="picright">
                <img src="../assets/xrpsm.png">
              </div>
            </button>
          </h5>
        </div>
        <div
          id="collapseThree"
          class="collapse"
          aria-labelledby="headingThree"
          data-parent="#accordian"
        >
          <div class="card-body">
            <div class="row">
              <div v-if="!xrpDist.rows">Loading...</div>
              <div v-else>
                <div
                  class="table-responsive"
                  style="margin-left: -6px; margin-top: -20px; width: 885px;"
                >
                  <table class="table table-striped">
                    <thead>
                      <tr class="colorrow2" style="width: 885px;">
                        <th class="text-left" scope="col">Date</th>
                        <th class="text-left" scope="col">Total</th>
                        <th class="text-left" scope="col">Distributed</th>
                        <th class="text-left" scope="col">Undistributed</th>
                        <th class="text-left" scope="col">Escrowed</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr
                        scope="row"
                        v-for="z in xrpDist.rows.slice().reverse()"
                        v-bind:key="z.date"
                      >
                        <td>{{ new Date(Date.parse(z.date)).toLocaleDateString('en-NZ',{ weekday: 'long', month: 'long', day: 'numeric', year: 'numeric'}).replace(',',' ') }}</td>
                        <td>{{ z.total }}</td>
                        <td>{{ z.distributed }}</td>
                        <td>{{ z.undistributed }}</td>
                        <td>{{ z.escrowed }}</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="card-header" id="headingFour" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapseFour"
              aria-expanded="false"
              aria-controls="collapseFour"
            >Validators - ALL</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapseFour"
              aria-expanded="false"
              aria-controls="collapseFour"
            >
              <div class="picright">
                <img src="../assets/xrpsm.png">
              </div>
            </button>
          </h5>
        </div>
        <div
          id="collapseFour"
          class="collapse"
          aria-labelledby="headingFour"
          data-parent="#accordian"
        >
          <div class="form-group"></div>

          <!--switch-->
          <!--switch end-->
          <div class="card-body">
            <div class="row">
              <div v-if="!validatorsData.validators">Loading...</div>
              <div v-else>
                <div
                  class="table-responsive"
                  style="margin-left: -6px; margin-top: -20px; width: 885px;"
                >
                  <table class="table table-striped">
                    <thead>
                      <tr class="colorrow2" style="width: 885px;">
                        <th class="text-left" scope="col">Public Key</th>
                        <th class="text-left" scope="col">Domain Name</th>
                        <th class="text-left" scope="col">UNL</th>
                        <th class="text-left" scope="col">
                          <span
                            title="The percentage of ledgers closed successfully measured against those closed by the default UNL validators"
                          >Score %</span>
                        </th>
                        <th class="text-left" scope="col">24Hour</th>
                        <th class="text-left" scope="col">Missed</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr scope="row" v-for="q in validatorsData.validators" v-bind:key="q.unl">
                        <td class="Tab4">
                          <a
                            v-bind:href="'https://xrpcharts.ripple.com/#/validators/' + q.validation_public_key"
                            target="_blank"
                            style="text-decoration:none;"
                          >{{ q.validation_public_key }}</a>
                        </td>
                        <td class="Tab4">
                          <p style="margin-bottom: 0;" v-if="q.domain"></p>
                          <p
                            style="margin-bottom: 0; color: darkblue; font-style: italic;"
                            v-else
                          >not yet verified</p>
                          <a
                            v-if="typeof q.domain !== 'undefined'"
                            v-bind:href="'http://' + q.domain"
                            target="_blank"
                            style="text-decoration:none;"
                          >{{ q.domain }}</a>
                        </td>
                        <td class="Tab4">
                          <p
                            style="text-align: center; color: darkgreen; margin-bottom: 0; font-size: 11px;"
                            v-if="q.unl"
                          >yes</p>
                        </td>
                        <td class="text-left" style="margin-bottom: 0; color: #007bff;">
                          <p
                            style="margin-bottom: 0;"
                            v-if="q.agreement_24h.score<1"
                          >{{ (q.agreement_24h.score * 100).toFixed(2) }}%</p>
                          <p style="margin-bottom: 0; color: darkblue;" v-else>100%</p>
                        </td>
                        <td
                          class="text-center"
                          style="margin-bottom: 0; color: #007bff;"
                        >{{ q.agreement_24h.total }}</td>
                        <td class="text-center" style="margin-bottom: 0; color: red;">
                          <p
                            style="margin-bottom: 0;"
                            v-if="q.agreement_24h.missed>0"
                          >{{ q.agreement_24h.missed }}</p>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!--end of Card 4-->
      </div>
    </div>
  </div>
</template>

<script>

export default {
 
  data: function() {
    return {
      
      //Rippled Lookup all validators
      appName: "AllVal",
      validatorsData: {},

      //Rippled Lookup all metrics
      appName: "xrpMetrics",
      xrpMetrics: {},

      //Rippled Lookup all fees
      appName: "xrpFees",
      xrpFees: {},

      //Rippled Lookup xrp distribution
      appName: "XRPDist",
      xrpDist: {},

      //Rippled Lookup xrp sales
      appName: "XRPSales",
      xrpData: {},

      //Rippled Lookup xrp distribution
      appName: "XRPBurn",
      xrpBurn: {}

    };
  }, 
  //--------------------------

  mounted() {
    
    //rippled Lookup all validators
    window
      .fetch("https://data.ripple.com/v2/network/validators/")
      .then(q => {
        return q.json();
      })
      .then(data => {
        // Set it to the Vue App data
        this.validatorsData = data;
      });

    //rippled Lookup all stats
    window
      .fetch("https://data.ripple.com/v2/stats?descending=true&limit=100")
      .then(p => {
        return p.json();
      })
      .then(data => {
        // Set it to the Vue App data
        this.xrpMetrics = data;
      });

    //rippled Lookup all fees
    window
      .fetch(
        "https://data.ripple.com/v2/network/fees?interval=day&descending=true&limit=100"
      )
      .then(d => {
        return d.json();
      })
      .then(data => {
        // Set it to the Vue App data
        this.xrpFees = data;
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

    //rippled lookup xrp distribution
    window
      .fetch("https://data.ripple.com/v2/network/xrp_distribution")
      .then(z => {
        return z.json();
      })
      .then(data => {
        // Set it to the Vue App data
        this.xrpDist = data;
      });
    //rippled lookup xrp distribution short
    window
      .fetch(
        "https://data.ripple.com/v2/network/xrp_distribution?descending=true&limit=1"
      )
      .then(k => {
        return k.json();
      })
      .then(data => {
        // Set it to the Vue App data
        this.xrpBurn = data;
      });
  },

  //end of mounted

};
</script>

