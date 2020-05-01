<template>
  <div
    class="cardouter tab-pane h-100 p-3 active rounded-bottom rounded-right"
    id="entry"
    role="tabpanel"
    aria-labelledby="entry-tab"
  >
    <!-- The above ACTIVE required for first TAB -->
    <!-- top of base info -->
    <div class="container-fluid" align="left" style="width: 860px; margin: 15px; height: auto;">
      <div style="margin-left:70px;">
        <div>
          <img class="whiteJPG" />
        </div>
        <table
          class="tab1Table table table-borderless md p-3 mb-5 shadow rounded"
          style="border-left: 0; border-right: 0; "
        >
          <thead>
            <tr class="theadTab1" style=" font-size:11px;">
              <!--font color in here -->
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
              :key="`idx-${a.date}`"
            >
              <td
                class="text-center"
                style="font-size: 12px;"
              >{{ new Date(Date.parse(a.date)).toLocaleDateString('en-NZ',{ month: 'long', day: 'numeric'}) }}</td>
              <td class="text-center" style="font-size: 11px;">{{ a.metric.ledger_count }}</td>
              <td
                class="text-center"
                style="font-size: 12px;"
              >{{ (a.metric.ledger_interval * 1).toFixed(5)}}</td>
              <td
                class="text-center"
                style="font-size: 12px;"
              >{{ a.metric.transaction_count.toLocaleString() }}</td>
              <td
                class="text-center"
                style="font-size: 12px;"
              >{{ (a.metric.tx_per_ledger * 1).toFixed(5)}}</td>
              <td class="text-center" style="font-size: 12px;">{{ a.metric.accounts_created }}</td>
              <td
                class="text-center"
                style="font-size: 12px;"
              >{{ ((xrpDataBrief.data.total* 1).toFixed(0)*1).toLocaleString() }} xrp</td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="col">
        <div align="center" style="margin-left: -25px;">
          <div class="chartsLayout">
            <p class="apexchartsHead">New XRPL Accounts Created - by Day - Last 45 Days</p>

            <RipCharts />
          </div>
          <div class="chartsLayout">
            <p class="apexchartsHead">New XRPL Accounts Created - by Week - Last 12 months</p>
            <RipChart1 />
          </div>
          <div class="chartsLayout">
            <p class="apexchartsHead">XRPL Total Transactions - by Day - Last 45 Days</p>

            <RipCharts2 />
          </div>
          <div
            class="disclaimer"
          >DISCLAIMER: All statistics quoted on this site may be subject to change without notice and no guarantee can be given as to their accuracy.</div>
        </div>
      </div>
    </div>
  </div>

  <!-- top of base info -->

  <!--end of Card 4-->

  <!-- End of Tab1e-->
</template>
<script>


Date.prototype.getWeek = function() {
  var d = new Date(
    Date.UTC(this.getFullYear(), this.getMonth(), this.getDate())
  );
  var dayNum = d.getUTCDay() || 7;
  d.setUTCDate(d.getUTCDate() + 4 - dayNum);
  var yearStart = new Date(Date.UTC(d.getUTCFullYear(), 0, 1));
  return Math.ceil(((d - yearStart) / 86400000 + 1) / 7);
};

//---------------------------------

import RipCharts from "./RipCharts"
import RipCharts2 from "./RipCharts2"
import RipChart1 from "./RipChart1"

export default {
  components: {
    RipChart1,
    RipCharts2,
    RipCharts
  },

  data: function() {
    return {
      //Rippled Lookup all metrics
      appName: "xrpMetricDay",
      xrpMetricDay: {},
      xrpDataBrief: {}
    };
  },

  mounted() {
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
      .then(n => {
        return n.json();
      })
      .then(data => {
        // Set it to the Vue App data
        this.xrpDataBrief = data;
      });
  }

  //end of methods
};
</script>
