<template>
  <div class="header">
    <div class="inner-header">
      <div class="logo-container">
        <div class="smallbig">
          <div>
            <div>rippleitin.nz&nbsp;&nbsp;</div>
          </div>

          <div>
            <div class="smallleft">
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
      <div class="xrpRates">
        <!--           <li>
           <div
              class="xumm-donate-button-no-border"
              style="width: 90px;
  position: absolute;
  top: 8px;   display: block;
    margin-left: -480px;
    margin-right: auto;">
      </div>
        </li>-->
        <a>
          <li>
            <p v-if="cryptos.XRP">
              Live
              <span class="xrpHead">XRP&thinsp;</span>
              prices
              <span
                v-for="(rate, coin) in cryptos.XRP"
                v-bind:key="`CIN-${coin}`"
              >&nbsp; &thinsp; {{ rate }}&thinsp;{{ coin }}</span>
            </p>
            <span v-else>Loading</span>
          </li>
        </a>
      </div>

      <div class="modeChange">
        <a>
          <li>
            <i class="fas fa-sun icon-sun" v-if="nightMode" @click="nightMode = !nightMode"></i>
            <i class="fas fa-sun icon-sun" v-else @click="nightMode =!nightMode"></i>
          </li>
          <li>
            <div
              class="xumm-donate-button-no-border"
              style="width: 90px;
  position: absolute;
  top: 155px;   display: block;
    margin-left: 350px;
    margin-right: auto;"
            ></div>
          </li>
        </a>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { stringify } from "querystring";

import "../css/styles.scss";
let apiInterval;

export default {
  name: "XRP",

  data: function() {
    return {
      //cryptoprices
      nightMode: null,
      cryptos: {
        XRP: []
      },
      //Rawdata from my validator
      serverState: null
    };
  },
  watch: {
    nightMode() {
      localStorage.setItem("nightMode", this.nightMode ? "1" : "0");
      document
        .querySelector("body")
        .setAttribute("class", this.nightMode ? "darkgold" : "daylight");
    }
  },
  mounted() {
    this.fetchRates();
    // Set interval at 60 seconds (re-run)
    apiInterval = setInterval(this.fetchRates, 60 * 1000);

    // Connect to server status
    this.serverStateHandler();
  },

  //end of mounted
  destroyed() {
    clearInterval(apiInterval);
  },

  created() {
    this.nightMode = false;
    try {
      this.nightMode = Boolean(Number(localStorage.getItem("nightMode")));
    } catch (ep) {}
  },

  //------------------------------------------------
  methods: {
    //rippled data from my server
    serverStateHandler() {
      const endpoint =
        process.env.NODE_ENV === "development"
          ? "ws://192.168.1.50:8181"
          : "wss://rippleitin.nz";
      const s = new WebSocket(endpoint);
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
        .catch(el => {
          window.console.log(el);
        });
    }
  }

  //end of methods
};
</script>
