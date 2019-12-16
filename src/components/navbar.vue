<template>
  <div id="navbar">
    

    <nav
      class="navbar navbar-expand-lg navbar-dark bg-dark fixed-bottom"
      style="border-top:5px solid white"
    >
      <a class="navbar-brand nav-link">
        <a
          class="nav-item"
          @click="clearHome(), display('home')"
          style="margin:10px; font-size:1em; color: white"
        >
          <i class="fas fa-home clickableAwesomeFont"></i>
        </a>
      </a>
      <div id="navbarSupportedContent" class="collapse navbar-collapse">
        <ul class="navbar-nav mx-auto">
          <li class="nav-item" 
          >
            <a v-if="last !== null" class="nav-link" href="https://cryptowat.ch/markets/bitstamp/xrp/usd/15m"
          target="_blank"
          >
              Bitstamp:
              <span class="gnlt">$ {{ last | toDecimal4 }}</span>
            </a>
            <a v-if="last === null" class="nav-link" href="https://cryptowat.ch/markets/bitstamp/xrp/usd/15m"
          target="_blank">
              Bitstamp:
              <span style="padding-left:5px" class="gnlt">
                <i class="fas fa-dollar-sign fa-spin"></i>
              </span>
            </a>
          </li>
          <li class="nav-item" style="pointer-events: none">
            <a class="nav-link">
              Tx Per Second:
              <span class="gnlt">{{ TPS | toNoDecimal }}</span>
            </a>
          </li>
          <li class="nav-item" style="pointer-events: none">
            <a class="nav-link">
              Avg Tx Count:
              <span class="gnlt">{{ avgTxnCt | toNoDecimal }}</span>
            </a>
          </li>
          <li class="nav-item" style="pointer-events: none">
            <a class="nav-link">
              Last Validated Ledger:
              <span class="gnlt">{{ lIndex |toNoDecimal }}</span>
            </a>
          </li>
          <li class="nav-item" style="pointer-events: none">
            <a class="nav-link">
              Transactions Included in Ledger:
              <span class="gnlt">{{ txnCt }}</span>
            </a>
          </li>
          
          
        </ul>
        <a
          class="nav-item"
          href="https://twitter.com/PadanaramDigi"
          target="_blank"
          style="margin:10px; font-size:1.25em; color: white"
        >
          <i class="fab fa-twitter"></i>
        </a>
      </div>
    </nav>
  </div>
</template>

<script>
//import RippleClient from 'rippled-ws-client';
import { EventBus } from "./eventbus.js";
import axios from "axios";
import Pusher from "pusher-js";
import RippleClient from "rippled-ws-client";
const pusher = new Pusher("de504dc5763aeef9ff52");
const channel = pusher.subscribe("live_trades_xrpusd");

export default {
  name: "navBar",
  data() {
    return {
      connection: null,
      lHash: null,
      lIndex: null,
      
      txnCt: null,
      ledger: null,

      avgTxnCt:null,
      TPS:null,
      ledgerAvgs:[],
      bitstamp: null,
      last: null,
      curLedgerSize: null,
      curQueSize: null,
      baseFee: null,
      medianFee: null,
      openLedgerFee: null,
      expectedLedgerSize: null,
      currentLedgerIndex: null
    };
  },

  mounted() {
    //  this.interval = setInterval(this.getPrice, 5000)
    channel.bind("trade", data => {
      this.last = data.price;
      EventBus.$emit("lastPrice", this.last);
    });
    EventBus.$on("ledgerCl", ledger => {
      this.ledger = ledger;
      this.getledgerCls();
    });
    new RippleClient("wss://padanaram.digital:8443")
      .then(RippledConnection => {
        this.connection = RippledConnection;
        this.interval = setInterval(this.getFee, 500);

        this.getFee();
      })
      .catch(error => {
        // Oops!
        alert("Oops!HelloWorld " + error.message);
      });
  },

  beforeDestroy() {
    clearInterval(this.interval);
  },

  //  beforeDestroy() {
  //    clearInterval(this.interval)
  //  },

  methods: {
    clearHome() {
      EventBus.$emit("home", "");
    },

    getledgerCls() {
      this.lHash = this.ledger.ledger_hash;
      this.lIndex = this.ledger.ledger_index;
      this.ledgerAvgs.push(this.ledger);
      if(this.ledgerAvgs.length>15){
        this.ledgerAvgs.shift();
      };
      let avgTxn = function (ledgerAvgs) {
        return ledgerAvgs.reduce((prev, ledgerAvg) => prev + ledgerAvg.txn_count, 0) / ledgerAvgs.length;
      };
      let avgTPS = function (ledgerAvgs) {
        return ledgerAvgs.reduce((prev, ledgerAvg) => prev + ledgerAvg.txn_count, 0) / (ledgerAvgs[ledgerAvgs.length-1].ledger_time -ledgerAvgs[0].ledger_time);
      };
      this.txnCt = this.ledger.txn_count;
      this.avgTxnCt = avgTxn(this.ledgerAvgs);
      this.TPS = avgTPS(this.ledgerAvgs);
    },

    getFee() {
      this.connection
        .send({
          id: "fee_websocket",
          command: "fee"
        })
        .then(response => {
          this.curLedgerSize = Number(response.current_ledger_size);
          this.curQueSize = response.current_queue_size;
          this.baseFee = response.drops.base_fee;
          this.medianFee = response.drops.median_fee;
          this.openLedgerFee = response.drops.open_ledger_fee;
          this.expectedLedgerSize = Number(response.expected_ledger_size);
          this.currentLedgerIndex = response.ledger_current_index;
        })
        .catch(error => {
          console.log("Oops!Acct Objects ", error.message);
        });
    },

    display: function(dview) {
      this.$parent.compdisplay = dview;
    }
  }
};
//#07e2ff--aqua blue color
//#088dfa - blue color
//#ed7717 --orange color
</script>


<style scoped>
.dropdown-toggle::after {
  display: none;
}

.logo {
  font-family: "Code-Light";
  font-weight: 800;
  font-size: 40px;
  text-shadow: 2px 2px 8px #07e2ff, -2px -2px 8px #07e2ff;
}

.waiting {
  color: orange;
}
.gnlt {
  color: white;
}
.acct {
  color: #8ebfff;
}
.dropdown-item {
  font-family: Lato, Arial, Tahoma, Verdana;
  font-size: 1em;
  line-height: 1.5em;
  color: #07e2ff;
  font-weight: 500;
  text-shadow: none;
}
.drp-list:hover {
  color: #07e2ff;
  background: #a1a1a1;
}

.nbgrad {
  background: rgb(21, 98, 171);
  background: linear-gradient(
    185deg,
    rgba(21, 98, 171, 1) 0%,
    rgba(4, 28, 84, 1) 100%
  );
}

.hiLoad {
  font-weight: 400;
  text-shadow: 2px 2px 5px #07e2ff, -2px -2px 5px #07e2ff;
}
</style>



// WEBPACK FOOTER //
// src/components/navbar.vue