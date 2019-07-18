<template>
  <!-- Tab One-->
  <div
    class="tab-pane h-100 p-3 border border-info rounded"
    id="XRPL"
    role="tabpanel"
    aria-labelledby="XRPL-tab"
    style="background-image: linear-gradient(white 30%, rgb(98, 189, 219)400%);"
  >
    <div class="card" style="width: 885px; margin: 15px;">
      <div id="accordian">
        <!--Card Metrics -->
        <div class="card-header" id="headingMetrics" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapseMetrics"
              aria-expanded="false"
              aria-controls="collapseMetrics"
            >XRP Ledger Metrics</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapseMetrics"
              aria-expanded="false"
              aria-controls="collapseMetrics"
            >
              <div class="picright">
                <img src="../assets/xrpsm.png">
              </div>
            </button>
          </h5>
        </div>

        <div
          id="collapseMetrics"
          class="collapse"
          aria-labelledby="headingMetrics"
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
                      <tr scope="row" v-for="p in xrpMetrics.stats" v-bind:key="`PPP-${p.date}`">
                        <td></td>
                        <td
                          class="text-left"
                        >{{ new Date(Date.parse(p.date)).toLocaleDateString('en-NZ',{ month: 'long', day: 'numeric', year: 'numeric'}).replace(',',' ') }}</td>
                        <td class="text-right">{{ p.metric.ledger_count.toLocaleString() }}</td>
                        <td class="text-right">{{ (p.metric.ledger_interval * 1).toFixed(5)}}</td>
                        <td class="text-right">{{ p.metric.transaction_count.toLocaleString() }}</td>
                        <td class="text-right">{{ (p.metric.tx_per_ledger * 1).toFixed(5)}}</td>
                        <td
                          class="text-right"
                        >{{ p.metric.accounts_created .toLocaleString() }} &nbsp; &nbsp;</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!--Card Week -->
        <div class="card-header" id="headingWeek" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapseWeek"
              aria-expanded="false"
              aria-controls="collapseWeek"
            >XRP Ledger Metrics by Week - Full History</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapseWeek"
              aria-expanded="false"
              aria-controls="collapseWeek"
            >
              <div class="picright">
                <img src="../assets/xrpsm.png">
              </div>
            </button>
          </h5>
        </div>

        <div
          id="collapseWeek"
          class="collapse"
          aria-labelledby="headingWeek"
          data-parent="#accordian"
        >
          <div class="card-body">
            <div class="row">
              <div v-if="!$parent.xrpMetricsAll.stats">Loading...</div>
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
                        <th class="text-center" scope="col">Total Accounts</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr scope="row" v-for="row in $parent.xrpMetricsAll.stats" :key="row.date">
                        <td></td>
                        <td
                          class="text-left"
                        >{{ new Date(Date.parse(row.date)).toLocaleDateString('en-NZ',{ month: 'long', day: 'numeric', year: 'numeric'}).replace(',',' ') }}</td>
                        <td class="text-right">{{ row.metric.ledger_count.toLocaleString() }}</td>
                        <td class="text-right">{{ (row.metric.ledger_interval * 1).toFixed(5)}}</td>
                        <td class="text-right">{{ row.metric.transaction_count.toLocaleString() }}</td>
                        <td class="text-right">{{ (row.metric.tx_per_ledger * 1).toFixed(5)}}</td>
                        <td
                          class="text-right"
                        >{{ row.metric.accounts_created.toLocaleString() }} &nbsp; &nbsp;</td>
                        <td class="text-right">{{ sumCreated(row).toLocaleString() }}&nbsp; &nbsp;</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!--Card Fees-->
        <div class="card-header" id="headingFees" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapseFees"
              aria-expanded="false"
              aria-controls="collapseFees"
            >XRP Ledger Fees</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapseFees"
              aria-expanded="false"
              aria-controls="collapseFees"
            >
              <div class="picright">
                <img src="../assets/xrpsm.png">
              </div>
            </button>
          </h5>
        </div>

        <div
          id="collapseFees"
          class="collapse"
          aria-labelledby="headingFees"
          data-parent="#accordian"
        >
          <div class="card-body">
            <div class="row">
              <div>
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
                        v-bind:key="`KKK-${k.date}`"
                      >
                        <td>{{ new Date(Date.parse(k.date)).toLocaleDateString('en-NZ',{ weekday: 'long', month: 'long', day: 'numeric', year: 'numeric'}).replace(',',' ') }}</td>
                        <td></td>
                        <td class="text-center">{{ (100000000000 - k.total).toLocaleString() }}</td>
                        <td></td>
                        <td class="text-center">{{ (k.total *1).toLocaleString() }}</td>
                        <td></td>
                        <td class="text-center">{{ ((100000000000 - k.total) / 100000000000) }}</td>
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
                      <tr scope="row" v-for="d in xrpFees.rows" v-bind:key="`DDD-${d.date}`">
                        <td></td>
                        <td
                          class="text-left"
                        >{{ new Date(Date.parse(d.date)).toLocaleDateString('en-NZ',{ month: 'long', day: 'numeric', year: 'numeric'}).replace(',',' ') }}</td>
                        <td class="text-right">{{ d.tx_count.toLocaleString() }}</td>
                        <td class="text-right">{{ d.min }}</td>
                        <td class="text-right">{{ d.max }}</td>
                        <td class="text-right">{{ d.avg }}</td>
                        <td class="text-right">{{ d.total }} &nbsp; &nbsp;</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- card four-->
        <div class="card-header" id="headingTransactions" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapseTransactions"
              aria-expanded="false"
              aria-controls="collapseTransactions"
            >XRP Transactions - Previous 24hrs - by Exchange</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapseTransactions"
              aria-expanded="false"
              aria-controls="collapseTransactions"
            >
              <div class="picright">
                <img src="../assets/xrpsm.png">
              </div>
            </button>
          </h5>
        </div>
        <div
          id="collapseTransactions"
          class="collapse"
          aria-labelledby="headingTransactions"
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
                        <td>{{ ((xrpData.data.total* 1).toFixed(0)*1).toLocaleString() }} xrp</td>
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
                         <th/>
                        <th class="text-left" scope="col">Exchange</th>
                        <th/>
                        <th class="text-right" scope="col">Volume</th>
                        <th/>
                        <th/>
                        <th class="text-right" scope="col">Trades</th>
                        <th/>
                        <th class="text-right" scope="col">Avg Trade</th>
                        <th/>
                        <th class="text-right" scope="col">Against</th>
                        <th/>
                      </tr>
                    </thead>
                    <tbody>
                      <tr
                        scope="row"
                        v-for="w in xrpData.data.components.slice().sort()"
                        v-bind:key="`WWW-${w.base_volume}`"
                      >
                        <td/>
                        <td class="text-left">{{ w.source }}</td>
                        <td/>
                        <td
                          class="text-right"
                        >{{ (((w.base_volume * 1).toFixed(5))*1).toLocaleString() }}</td>
                        <td/>
                        <td/>
                        <td class="text-right">{{ w.count.toLocaleString() }}</td>
                        <td/>
                        <td
                          class="text-right"
                        >{{ ((w.base_volume / w.count).toFixed(5)).toLocaleString() }}</td>
                        <td/>
                        <td class="text-right">{{ w.counter_currency }}</td>
                        <td/>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Card 5 Distribution-->
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
                        v-bind:key="`ZZZ-${z.date}`"
                      >
                        <td>{{ new Date(Date.parse(z.date)).toLocaleDateString('en-NZ',{ weekday: 'long', month: 'long', day: 'numeric', year: 'numeric'}).replace(',',' ') }}</td>
                        <td>{{ (z.total*1).toLocaleString() }}</td>
                        <td>{{ (z.distributed*1).toLocaleString() }}</td>
                        <td>{{ (z.undistributed*1).toLocaleString()}}</td>
                        <td>{{ (z.escrowed*1).toLocaleString() }}</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!--Card 6 Validators-->
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
                      <tr
                        scope="row"
                        v-for="q in validatorsData.validators"
                        v-bind:key= "q.unl"
                      >
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
                            v-if= "q.unl"
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
      appName: "xrpAllVal",
      validatorsData: {},

      xrpMetrics: {},

      xrpFees: {},

      xrpDist: {},

      xrpData: {},

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

    //rippled Lookup all stats by day (100)
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

  methods: {
    sumCreated(row) {
      // No need to build a new array or total in a global,
      // we'll just lookup the current index in the stats array (indexOf)
      const currentRowIndex = this.$parent.xrpMetricsAll.stats.indexOf(row);
      return this.$parent.xrpMetricsAll.stats
        .slice(currentRowIndex)
        .reduce((a, b) => {
          return a + b.metric.accounts_created;
        }, 0);
    }
  },

  created() {
    window
      .fetch(
        "https://data.ripple.com/v2/stats?descending=true&interval=week&limit=400"
      )
      .then(x => {
        return x.json();
      })
      .then(data => {
        // Set it to the Vue App data
        this.$parent.xrpMetricsAll = Object.assign(data, {
          // Use data, but overwrite the stats element with a sorted version
          stats: data.stats
            .sort((a, b) => {
              if (a.date > b.date) return -1;
              if (a.date < b.date) return 1;
              return 0;
            })
            .map(a => {
              // Chain a mapper, convert all date fields to actual JS date objects,
              // so we can call date methods in the Vue template on the date field
              return Object.assign(a, {
                date: new Date(a.date)
              });
            })
        });
      });
  }
};
</script>

