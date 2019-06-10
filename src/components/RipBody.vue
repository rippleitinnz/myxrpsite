<template>
  <div id="app">
   
  <div>
   <div class="margtop">
        <div class="container-fluid" align="center" style="width: 950px; margin-bottom: 60px;">
          <br>
          <ul class="nav nav-tabs border-0" id="myTab" role="tablist">
            <li class="nav-item">
              <a
                class="nav-link active border border-info border-bottom-0"
                id="entry-tab"
                data-toggle="tab"
                href="#entry"
                role="tab"
                aria-controls="entry"
                aria-selected="true"
              >the XRPL today...</a>
            </li>
            <li class="nav-item">
              <a
                class="nav-link border border-info border-bottom-0"
                id="XRPL-tab"
                data-toggle="tab"
                href="#XRPL"
                role="tab"
                aria-controls="XRPL"
                aria-selected="false"
              >XRPL Metrics</a>
            </li>
            <li class="nav-item">
              <a
                class="nav-link border border-info border-bottom-0"
                id="rippleitinnz-tab"
                data-toggle="tab"
                href="#rippleitinnz"
                role="tab"
                aria-controls="rippleitinnz"
                aria-selected="false"
              >This Validator</a>
            </li>
            <li class="nav-item">
              <a
                class="nav-link border border-info border-bottom-0"
                id="links-tab"
                data-toggle="tab"
                href="#links"
                role="tab"
                aria-controls="links"
                aria-selected="false"
              >Essential Links</a>
            </li>
          </ul>

          <div class="tab-content h-75">
<!-- Tab Components -->

          <RipTabOne />
          <RipTabTwo />
          <RipTabThree />
          <RipTabFour />

<!-- Tab Componenets End -->
        </div>
        </div>
        </div>
        </div>
          <RipCharts />
        </div>
      

</template>

<script>
import axios from "axios";
import RipTabOne from './RipTabOne';
import RipTabTwo from './RipTabTwo';
import RipTabThree from './RipTabThree';
import RipTabFour from './RipTabFour';
import RipCharts from './RipCharts';

// charts
import VueApexCharts from "vue-apexcharts";
let apiInterval;
let apiIntervals;
//---------------------------------
export default {
  name: "XRP",

  name: "sXRP",
  // charts
  name: "Vue Chart",
  components: { 
    VueApexCharts,
    RipTabOne,
    RipTabTwo,
    RipTabThree,
    RipTabFour,
    RipCharts
     },
  //----------------------------------
  data: function() {
    return {
      //chart
      options: {
        chart: {
          id: "vuechart-example"
        },
        xaxis: {
          categories: [1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998]
        }
      },
      series: [
        {
          name: "series-1",
          data: [10, 20, 30, 40, 50, 60, 70, 90]
        }
      ],

      //cryptoprices
      cryptos: {
        XRP: {}
      },
      //cryptoprices
      scryptos: {
        sXRP: {}
      },

      //Rawdata from my validator
      serverState: null,

      //Rippled Lookup my validator
      appName: "SampleApp",
      validatorData: {},

      //Rippled Lookup all validators
      appName: "ValApp",
      validatorsData: {},

      //Rippled Lookup all metrics
      appName: "xrpMetrics",
      xrpMetrics: {},

      //Rippled Lookup all fees
      appName: "xrpFees",
      xrpFees: {},

      //Rippled Lookup all metrics
      appName: "xrpMetricsDay",
      xrpMetricDay: {},

      //Rippled Lookup xrp distribution
      appName: "XRPDist",
      xrpDist: {}

      /*     //Messages
      newMessage: "",
      message: "",
      cursor: "",
      messages: [
        "is the digital asset for payments",
        "was built for enterprise use",
        "is frictionless",
        "is fast, settling in just 4 seconds",
        "provides an on demand option to source liquidity",
        "has purpose, acting as a bridge currency",
        "is scalable beyond 1500 transactions per second",
        "is stable - all 40 million ledgers closed without issue",
        "is universally distributed and decentralised",
        "enables cross-border payments in seconds",
        "is the future of money, today",
        "is the Internet of Value"
      ] 
   */
    };
  }, //end of return
  //end of data
  //--------------------------

  mounted() {
    //  this.scrollText();

    // Run rates immediately

    this.fetchRates();
    // Set interval at 20 seconds (re-run)
    apiInterval = setInterval(this.fetchRates, 20 * 1000);

    // Run Srates immediately

    this.fetchSRates();

    // Set interval at 20 seconds (re-run)
    apiIntervals = setInterval(this.fetchSRates, 20 * 1000);
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
  destroyed() {
    clearInterval(apiInterval);
  },
  destroyed() {
    clearInterval(apiIntervals);
  },

  //------------------------------------------------
  methods: {
    //message scroller

    /*   removeMessage(message) {
        this.messages.splice(this.messages.indexOf(message), 1);
      },
      addMessage() {
        if (this.newMessage.trim() !== "") {
          this.messages.push(this.newMessage);
          this.newMessage = "";
        }
      },
      scrollText() {
        setInterval(() => {
          this.cursor = this.cursor === "|" ? "" : "|";
        }, 450); //cursor flash speed
        let iteration = 0;
        let interval;
        const run = () => {
          if (iteration > this.messages.length - 1) iteration = 0;
          this.message = "";
          interval = setInterval(() => {
            this.message = this.messages[iteration].slice(
              0,
              this.message.length + 2
            );
            if (this.message.length === this.messages[iteration].length) {
              clearInterval(interval);
              iteration++;
              setTimeout(run, 2500); // 2.5 sec pause then next sentence
            }
          }, 25); // 1 char every .1 sec
        };
        run();
      },
  */

    /*    // chart rebuild button
      updateChart() {
        const max = 90;
        const min = 20;
        const newData = this.series[0].data.map(() => {
          return Math.floor(Math.random() * (max - min + 1)) + min;
        });
        this.series = [
          {
            data: newData
          }
        ];
      },
*/
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
    },
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
        .catch(f => {
          window.console.log(f);
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