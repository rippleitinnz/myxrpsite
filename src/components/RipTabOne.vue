<template>
  <div
    class="tab-pane h-100 p-3 active border border-info rounded-bottom rounded-right"
    id="entry"
    role="tabpanel"
    aria-labelledby="entry-tab"
    style="background-image: linear-gradient(white 30%, rgb(98, 189, 219)400%);"
  >
    <!-- top of base info -->
    <div
      class="container-fluid"
      align="left"
      style="width: 860px; margin-top: 25px; height: auto;"
    >
      <div style="margin-left:70px;">
        <div>
          <img
            src="../assets/black.jpg"
            class="float-left"
            style="margin-left: -70px; width: 8%; height: auto; margin-top: 10px; "
          >
        </div>
        <table
          class="table table-borderless md p-3 mb-5 shadow bg-light rounded"
          style="border-left: 0; border-right: 0; "
        >
          <thead>
            <tr style="color: darkblue; font-size:12px;">
              <th class="text-center" scope="col">Date</th>
              <th class="text-center" scope="col">Ledger Count</th>
              <th class="text-center" scope="col">Ledger Interval</th>
              <th class="text-center" scope="col">Total Transactions</th>

              <th class="text-center" scope="col">TX per Ledger</th>
              <th class="text-center" scope="col">New Accounts</th>
              <th class="text-center" scope="col">Exchange Volume</th>
            </tr>
          </thead>
          <tbody>
            <tr
              scope="row"
              v-for="a in xrpMetricDay.stats.slice().reverse().splice(index,1)"
              v-bind:key="a.date"
            >
              <td
                class="text-center"
                style="font-size: 13px;"
              >{{ new Date(Date.parse(a.date)).toLocaleDateString('en-NZ',{ month: 'long', day: 'numeric'}) }}</td>
              <td class="text-center" style="font-size: 11px;">{{ a.metric.ledger_count }}</td>
              <td
                class="text-center"
                style="font-size: 13px;"
              >{{ (a.metric.ledger_interval * 1).toFixed(5)}}</td>
              <td class="text-center" style="font-size: 13px;">{{ a.metric.transaction_count }}</td>
              <td
                class="text-center"
                style="font-size: 13px;"
              >{{ (a.metric.tx_per_ledger * 1).toFixed(5)}}</td>
              <td class="text-center" style="font-size: 13px;">{{ a.metric.accounts_created }}</td>
              <td
                class="text-center"
                style="font-size: 13px;"
              >{{ (xrpData.data.total* 1).toFixed(0) }} xrp</td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="col">
        <div align="center" style="font-size: 1.75em;margin-left: -20px;">
<p style="font-size: 16px; color: darkblue;">New Accounts Created</p>
          <RipCharts/>
         
        </div>
      </div>
    </div>
    <!-- top of base info -->
  </div>

  <!--end of Card 4-->

  <!-- End of Tab1-->
</template>
<script>
import axios from "axios";
import RipCharts from "./RipCharts";
let apiIntervals;
//---------------------------------
export default {
  name: "sXRP",
   components: {
       RipCharts
  },

  data: function() {
    return {
      //cryptoprices
      scryptos: {
        sXRP: {}
      },

      //Rippled Lookup all metrics
      appName: "xrpMetricsDay",
      xrpMetricDay: {},
      xrpData: {}
    };
  },

  mounted() {
    this.fetchSRates();

    // Set interval at 20 seconds (re-run)
    apiIntervals = setInterval(this.fetchSRates, 20 * 1000);

    //rippled Lookup all stats
    window
      .fetch("https://data.ripple.com/v2/stats?descending=true&limit=2")
      .then(a => {
        return a.json();
      })
      .then(data => {
        // Set it to the Vue App data
        this.xrpMetricDay = data;
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
  },

  //end of mounted
  destroyed() {
    clearInterval(apiIntervals);
  },

  //------------------------------------------------
  methods: {
    //rates data short
    fetchSRates() {
      axios
        .get(
          "https://min-api.cryptocompare.com/data/pricemulti?fsyms=XRP&tsyms=USD"
        )
        .then(response => {
          this.scryptos = response.data;
          window.console.log(response);
        })
        .catch(w => {
          window.console.log(w);
        });
    }
  }

  //end of methods
};
</script>
