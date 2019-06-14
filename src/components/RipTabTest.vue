<template>
  <!-- Tab One-->
  <div
    class="tab-pane h-100 p-3 border border-info rounded"
    id="testing"
    role="tabpanel"
    aria-labelledby="testing"
  >
    <div class="card" style="width: 885px; margin-top: 25px;">
      <div id="accordian">
        <!--Card Week -->
        <div class="card-header" id="headingTesting" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapseTesting"
              aria-expanded="false"
              aria-controls="collapseTesting"
            >XRP Ledger Metrics by Week - Full History</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapseTesting"
              aria-expanded="false"
              aria-controls="collapseTesting"
            >
              <div class="picright">
                <img src="../assets/xrpsm.png">
              </div>
            </button>
          </h5>
        </div>

        <div
          id="collapseTesting"
          class="collapse"
          aria-labelledby="headingTesting"
          data-parent="#accordian"
        >
          <div class="card-body">
            <div class="row">
              <div v-if="!xrpAlso.stats">Loading...</div>
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
                      <tr
                        scope="row"
                        v-for="mat in xrpAlso.stats"
                        :key="mat.date"
                      >
                        <td></td>
                        <td
                          class="text-left"
                        >{{ new Date(Date.parse(mat.date)).toLocaleDateString('en-NZ',{ month: 'long', day: 'numeric', year: 'numeric'}).replace(',',' ') }}</td>
                        <td class="text-right">{{ mat.metric.ledger_count }}</td>
                        <td class="text-right">{{ (mat.metric.ledger_interval * 1).toFixed(5)}}</td>
                        <td class="text-right">{{ mat.metric.transaction_count }}</td>
                        <td class="text-right">{{ (mat.metric.tx_per_ledger * 1).toFixed(5)}}</td>
                        <td class="text-right">{{ mat.metric.accounts_created }} &nbsp; &nbsp;</td>
                        <td
                          class="text-right"
                        >{{ total(mat.metric.accounts_created) }}&nbsp; &nbsp;</td>
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
</template>

<script>
let runningTotal = [];
export default {
  data: function() {
    return {
      //Rippled Lookup all validators
      appName: "xrpAllVal",

      xrpAlso: {}
    };
  },
  //--------------------------
  methods: {
    total(val) {
      runningTotal.push(val);

      return runningTotal.reduce((acc, curr) => acc + curr, 0);
    }
  },
 //--------------------------
  mounted() {
    window
      .fetch("https://data.ripple.com/v2/stats?interval=week&limit=5")
      .then(stat => {
        return stat.json();
      })
      .then(data => {
        // Set it to the Vue App data
        this.xrpAlso = data;
      });
  }

  //end of mounted
};
</script>

