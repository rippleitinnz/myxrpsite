<template>
  <div id="app">
    <div class="container-fluid">
      <div class="header">
        <div class="row" style="margin-right: 5px;">
          <div class="smallbig" style="width: 48.5%;">
            <div>
              <div>rippleitin.nz &nbsp;</div>
            </div>

            <div>
              <div class="smallleft">
                <div>
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
          <!--   -->
          <div style="font-size: 1rem; align: center; margin-top: 5px;">
            <a
              href="https://twitter.com/rippleitinNZ"
              target="_blank"
              style="text-decoration:none;"
              class="icoTwitter"
              title="find us on Twitter"
            >
              <i class="fab fa-twitter"></i>
            </a>
          </div>

          <div class="col">
            <div class="smallest" align="right">
              <span>
                <p v-if="cryptos.XRP" style="color: black;">
                  Live
                  <a style="color: rgb(56, 86, 141);">XRP&nbsp;</a>prices
                  <span
                    style="color: black;"
                    v-for="(rate, coin) in cryptos.XRP"
                    v-bind:key="coin"
                  >&nbsp; &nbsp; {{ rate }}&thinsp;{{ coin }}</span>
                </p>
                <span v-else>Loading</span>
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>

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
            <!--    <li class="nav-item">
              <a
                class="nav-link border border-info border-bottom-0"
                id="Scams-tab"
                data-toggle="tab"
                href="#Scams"
                role="tab"
                aria-controls="Scams"
                aria-selected="false"
              >Scam Alerts</a>
            </li>
            <li class="nav-item">
              <a
                class="nav-link border border-info border-bottom-0"
                id="donate-tab"
                data-toggle="tab"
                href="#donate"
                role="tab"
                aria-controls="donate"
                aria-selected="false"
              >Donate</a>
            </li>-->
          </ul>

          <div class="tab-content h-75">
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
                style="width: 860px; margin-top: 25px; height: 250px;"
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
                        <td
                          class="text-center"
                          style="font-size: 13px;"
                        >{{ a.metric.transaction_count }}</td>
                        <td
                          class="text-center"
                          style="font-size: 13px;"
                        >{{ (a.metric.tx_per_ledger * 1).toFixed(5)}}</td>
                        <td
                          class="text-center"
                          style="font-size: 13px;"
                        >{{ a.metric.accounts_created }}</td>
                        <td
                          class="text-center"
                          style="font-size: 13px;"
                        >{{ (xrpData.data.total* 1).toFixed(0) }} xrp</td>
                      </tr>
                    </tbody>
                  </table>
                </div>

                <div class="col">
                  <div class="smallest" align="center" style="font-size: 1.75em; margin-top: 80px;">
                    <span>
                      <p v-if="scryptos.XRP" style="color: darkblue;">
                        <span
                          style="color: blue;"
                          v-for="(rate, coin) in scryptos.XRP"
                          v-bind:key="coin"
                        >&nbsp; &nbsp; {{ rate }}&thinsp;</span>
                        <span style="font-size: .5em;">USD</span>
                      </p>
                      <span v-else>Loading</span>
                    </span>
                  </div>
                </div>
              </div>
              <!-- top of base info -->
            </div>

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
                        <div v-if="!c">Loading...</div>
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
                                  <td
                                    class="text-right"
                                  >{{ (p.metric.ledger_interval * 1).toFixed(5)}}</td>
                                  <td class="text-right">{{ p.metric.transaction_count }}</td>
                                  <td
                                    class="text-right"
                                  >{{ (p.metric.tx_per_ledger * 1).toFixed(5)}}</td>
                                  <td
                                    class="text-right"
                                  >{{ p.metric.accounts_created }} &nbsp; &nbsp;</td>
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
                                  <td
                                    class="text-center"
                                  >{{ (100000000000 - k.total) / 100000000000 }}</td>
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

                  <!-- Card four-->
                  <!--
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
                                  v-bind:key="q.unl"
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
                  </div>-->
                  <!--end of Card 4-->
                  <!-- Card four-->
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
                                <tr
                                  scope="row"
                                  v-for="q in validatorsData.validators"
                                  v-bind:key="q.unl"
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
            <!-- End of Tab1-->
            <!-- Tab2 Card 5-->
            <div
              class="tab-pane h-100 p-3 border border-info rounded"
              id="rippleitinnz"
              role="tabpanel"
              aria-labelledby="rippleitinnz-tab"
            >
              <div class="card" style="width: 885px;margin-top: 25px;">
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
                          <img src="../assets/xrpsm.png">
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
                      <div class="row">
                        <div class="serverst d-none d-sm-block col-sm-3 text-right">Server State</div>
                        <div
                          class="serverst d-block d-sm-none col-12 text-left text-muted"
                        >Server State</div>
                        <div class="serverrt col-12 col-sm-9">
                          <b v-if="serverState !== null">
                            <span class="text-success">{{ serverState.server_state }}</span>
                          </b>
                        </div>

                        <div class="serverst d-none d-sm-block col-sm-3 text-right">IO Latency</div>
                        <div
                          class="serverst d-block d-sm-none col-12 text-left text-muted"
                        >IO Latency</div>
                        <div class="serverrt col-12 col-sm-9">
                          <span v-if="serverState !== null">{{ serverState.io_latency_ms}}ms</span>
                        </div>

                        <div class="serverst d-none d-sm-block col-sm-3 text-right">Build version</div>
                        <div
                          class="serverst d-block d-sm-none col-12 text-left text-muted"
                        >Build version</div>
                        <div class="serverrt col-12 col-sm-9">
                          <span v-if="serverState !== null">{{ serverState.build_version }}</span>
                        </div>

                        <div class="serverst d-none d-sm-block col-sm-3 text-right">Server Uptime</div>
                        <div
                          class="serverst d-block d-sm-none col-12 text-left text-muted"
                        >Server Uptime</div>
                        <div class="serverrt col-12 col-sm-9">
                          <span
                            v-if="serverState !== null"
                          >{{ Math.round(serverState.uptime/86400 * 100)/ 100}} days</span>
                        </div>

                        <div class="serverst d-none d-sm-block col-sm-3 text-right">Peers</div>
                        <div class="serverst d-block d-sm-none col-12 text-left text-muted">Peers</div>
                        <div class="serverrt col-12 col-sm-9">
                          <span v-if="serverState !== null">{{ serverState.peers }}</span>
                        </div>

                        <div class="serverst d-none d-sm-block col-sm-3 text-right">Last ledger</div>
                        <div
                          class="serverst d-block d-sm-none col-12 text-left text-muted"
                        >Last ledger</div>
                        <div class="serverrt col-12 col-sm-9">
                          <b v-if="serverState !== null">{{ serverState.validated_ledger.seq }}</b>
                        </div>

                        <div class="serverst d-none d-sm-block col-sm-3 text-right">Complete Ledgers</div>
                        <div
                          class="serverst d-block d-sm-none col-12 text-left text-muted"
                        >Complete Ledgers</div>
                        <div class="serverrt col-12 col-sm-9">
                          <span v-if="serverState !== null">{{ serverState.complete_ledgers }}</span>
                        </div>

                        <div class="serverst d-none d-sm-block col-sm-3 text-right">Last Ledger #</div>
                        <div
                          class="serverst d-block d-sm-none col-12 text-left text-muted"
                        >Last Ledger #</div>
                        <div class="serverrt col-12 col-sm-9">
                          <span v-if="serverState !== null">{{ serverState.validated_ledger.hash }}</span>
                        </div>

                        <div class="serverst d-none d-sm-block col-sm-3 text-right">Public Key</div>
                        <div
                          class="serverst d-block d-sm-none col-12 text-left text-muted"
                        >Public Key</div>
                        <div class="serverrt col-12 col-sm-9">
                          <span>nHUcQnmEbCNq4uhntFudzfrZV8P5WLoBrR5h3R9jAd621Aaz1pSy</span>
                          <!--<span v-if="serverState !== null">{{ serverState.pubkey_node }}</span>-->
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
                          <img src="../assets/xrpsm.png">
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
                        <div
                          style="font-size: 1.1em; text-align: left;"
                          v-if="serverState !== null"
                        >
                          <pre>{{ serverState }}</pre>
                          <br>
                          <br>
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
                          <img src="../assets/xrpsm.png">
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
                        <div
                          class="serverst d-block d-sm-none col-12 text-left text-muted"
                        >Motherboard</div>
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
                        <div
                          class="serverst d-block d-sm-none col-12 text-left text-muted"
                        >Server Location</div>
                        <div class="serverrt col-12 col-sm-9">
                          <span>New Zealand</span>
                        </div>

                        <div
                          class="serverst d-none d-sm-block col-sm-3 text-right"
                        >Internet Connectivity</div>
                        <div
                          class="serverst d-block d-sm-none col-12 text-left text-muted"
                        >Internet Connectivity</div>
                        <div class="serverrt col-12 col-sm-9">
                          <span>Gigabit Fibre with 4G Cellular redundancy</span>
                        </div>

                        <div
                          class="serverst d-none d-sm-block col-sm-3 text-right"
                        >Server Admin Contact</div>
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
                          <img src="../assets/xrpsm.png">
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
                                  v-bind:key="r.date"
                                >
                                  <td></td>
                                  <td>{{ new Date(Date.parse(r.date)).toLocaleDateString('en-NZ',{ weekday: 'long', month: 'long', day: 'numeric', year: 'numeric'}).replace(',',' ') }}</td>
                                  <td class="text-right">{{ r.total }}</td>
                                  <td class="text-right">
                                    <p
                                      style="color: red; margin-bottom: 0;"
                                      v-if="r.missed>0"
                                    >{{ r.missed }}</p>
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
                </div>

                <!--  <div class="tab-pane h-100 p-3 border border-info" id="settings" role="tabpanel" aria-labelledby="settings-tab">Settings tab content...</div>-->
              </div>
            </div>
            <!--Tab 3 No Cards-->

            <div
              class="tab-pane h-100 p-3 border border-info rounded"
              id="links"
              role="tabpanel"
              aria-labelledby="links-tab"
            >
              <div class="card" style="width: 885px;margin-top: 25px;">
                <div class="photo">
                  <div style="text-align: left; margin: 30px 0px 30px 30px; font-size: 16px;">
                    <div class="row">
                      <div style="margin-top: 3px; margin-left: 30px;">
                        <div class="fa-xs">
                          <i class="fas fa-cog fa-spin"></i>
                        </div>
                      </div>

                      <div style="margin-left:30px;">
                        The Busy Persons Guide to
                        <b>
                          <i>XRP&nbsp;</i>
                        </b>(and Ripple) by
                        <a
                          href="https://xrpcommunity.blog/ripple-xrp-101/"
                          target="_blank"
                          style="text-decoration:none;"
                        >Tiffany Hayden</a>
                      </div>
                    </div>
                    <br>

                    <div class="row">
                      <div style="margin-top: 3px; margin-left: 30px;">
                        <div class="fa-xs">
                          <i class="fas fa-cog fa-spin"></i>
                        </div>
                      </div>
                      <div style="margin-left:30px;">
                        An authoritive blog on all things
                        <b>
                          <i>XRPL&nbsp;</i>
                        </b>and Ripple by
                        <a
                          href="https://xrpcommunity.blog/author/hodor/"
                          target="_blank"
                          style="text-decoration:none;"
                        >HODOR</a>
                      </div>
                    </div>
                    <br>

                    <div class="row">
                      <div style="margin-top: 3px; margin-left: 30px;">
                        <div class="fa-xs">
                          <i class="fas fa-cog fa-spin"></i>
                        </div>
                      </div>
                      <div style="margin-left:30px;">
                        <b>
                          <i>Web Monetization&nbsp;</i>
                        </b>- the browser API that uses ILP micropayments to monetize a web-site by
                        <a
                          href="https://coil.com"
                          target="_blank"
                          style="text-decoration:none;"
                        >@Coil</a>
                      </div>
                    </div>
                    <br>

                    <div class="row">
                      <div style="margin-top: 3px; margin-left: 30px;">
                        <div class="fa-xs">
                          <i class="fas fa-cog fa-spin"></i>
                        </div>
                      </div>
                      <div style="margin-left:30px;">
                        The infamous
                        <a
                          href="https://www.xrptipbot.com/"
                          target="_blank"
                          style="text-decoration:none;"
                        >XRP-Tipbot</a>
                        for reddit, Twitter and Discord by
                        <i>
                          <b>Wietse Wind</b>
                        </i>
                      </div>
                    </div>
                    <br>

                    <div class="row">
                      <div style="margin-top: 3px; margin-left: 30px;">
                        <div class="fa-xs">
                          <i class="fas fa-cog fa-spin"></i>
                        </div>
                      </div>
                      <div style="margin-left:30px;">
                        <b>
                          <i>XRP Tools&nbsp;</i>
                        </b>- by Bithomp - providing an easy gateway to query or create on the XRPL
                        <a
                          href="https://bithomp.com/"
                          target="_blank"
                          style="text-decoration:none;"
                        >XRP Tools</a>
                      </div>
                    </div>
                    <br>

                    <div class="row">
                      <div style="margin-top: 3px; margin-left: 30px;">
                        <div class="fa-xs">
                          <i class="fas fa-cog fa-spin"></i>
                        </div>
                      </div>
                      <div style="margin-left:30px;">
                        The innovators -
                        <b>
                          <i>XRPL-Labs&nbsp;</i>
                        </b> develops apps for the XRP Ledger
                        <a
                          href="https://xrpl-labs.com/en/"
                          target="_blank"
                          style="text-decoration:none;"
                        >Tristan, Ali and Wietse</a>
                      </div>
                    </div>
                    <br>

                    <div class="row">
                      <div style="margin-top: 3px; margin-left: 30px;">
                        <div class="fa-xs">
                          <i class="fas fa-cog fa-spin"></i>
                        </div>
                      </div>
                      <div style="margin-left:30px;">
                        <b>
                          <i>Other great blogs&nbsp;</i>
                        </b>by the
                        <a
                          href="https://xrpcommunity.blog/"
                          target="_blank"
                          style="text-decoration:none;"
                        >XRP Community</a>
                      </div>
                    </div>
                    <br>

                    <div class="row">
                      <div style="margin-top: 3px; margin-left: 30px;">
                        <div class="fa-xs">
                          <i class="fas fa-cog fa-spin"></i>
                        </div>
                      </div>
                      <div style="margin-left:30px;">
                        <a href="https://ripple.com" target="_blank" style="text-decoration:none;">
                          <b>
                            <i>Ripple</i>
                          </b>
                        </a> - the company
                      </div>
                    </div>
                    <br>

                    <div class="row">
                      <div style="margin-top: 3px; margin-left: 30px;">
                        <div class="fa-xs">
                          <i class="fas fa-cog fa-spin"></i>
                        </div>
                      </div>
                      <div style="margin-left:30px;">
                        <a
                          href="https://developers.ripple.com/build-run-rippled-ubuntu.html"
                          target="_blank"
                          style="text-decoration:none;"
                        >
                          <b>
                            <i>Run an XRPL Validator</i>
                          </b>
                        </a>
                      </div>
                    </div>
                    <br>
                  </div>
                </div>
              </div>
            </div>

            <!--Tab 4 Scams card nine -->

            <div
              class="tab-pane h-100 p-3 border border-info"
              id="Scams"
              role="tabpanel"
              aria-labelledby="Scams-tab"
            >
              <div class="card" style="width: 885px;margin-top: 25px;">
                <div id="accordian3">
                  <div class="card-header" id="headingNine" align="left">
                    <h5 class="mb-0">
                      <button
                        class="btn btn-link collapsed"
                        data-toggle="collapse"
                        data-target="#collapseNine"
                        aria-expanded="false"
                        aria-controls="collapseNine"
                      >Scams</button>
                      <button
                        class="btn btn-link collapsed float-right"
                        data-toggle="collapse"
                        data-target="#collapseNine"
                        aria-expanded="false"
                        aria-controls="collapseNine"
                      >
                        <div class="picright">
                          <img src="../assets/xrpsm.png">
                        </div>
                      </button>
                    </h5>
                  </div>

                  <div
                    id="collapseNine"
                    class="collapse"
                    aria-labelledby="headingNine"
                    data-parent="#accordian3"
                  >
                    <div class="card-body">
                      Coming Soon!
                      <!-- holding      <div class="row">
                        <div v-if="!xrpDupe">Loading...</div>
                        <div v-else>
                          <div
                            class="table-responsive"
                            style="margin-left: -6px; margin-top: -20px; width: 885px;"
                          >
                            <table class="table table-striped">
                              <thead>
                                <tr class="colorrow2" style="width: 885px;">
                                  <th class="text-left" scope="col">Wallet</th>
                                  <th class="text-left" scope="col">Status</th>
                                  <th class="text-left" scope="col">Domain</th>
                                 
                                </tr>
                              </thead>
                              <tbody>
                                <tr
                                  scope="row"
                                  v-for="b in xrpDupe"
                                  v-bind:key="b.address"
                                >
                                  <td>{{ b.address}}</td>
                                  <td>{{ b.status }}</td>
                                  <td>{{ b.domain }}</td>
                                  
                                </tr>
                              </tbody>
                            </table>
                          </div>
                        </div>
                      </div> 
                      holding-->
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <!--Tab 4 Donations no cards -->

            <!--     <div
              class="tab-pane h-100 p-3 border border-info"
              id="donate"
              role="tabpanel"
              aria-labelledby="donate-tab"
            >
              <div class="card" style="width: 885px;margin-top: 25px;">
                <div style="text-align: left; margin: 30px 0px 30px 30px; font-size: 16px;">
                  <div>
                    <VueApexCharts width="500px" type="bar" :options="options" :series="series"></VueApexCharts>
                    <br>
                    <button @click="updateChart">Update chart</button>
                  </div>
                  <br>
                </div>
              </div>
            </div>-->
          </div>
          <!-- INSERT 11-->
        </div>
      </div>
    </div>
    <!--
    <div>
      <VueApexCharts width="600" type="bar" :options="options" :series="series"></VueApexCharts>
    </div>-->
    <!--footer-->

    <!--footer-->

    <div class="footer">
      <div class="col" style="justify-content: left; margin-left: 8px; margin-bottom: 45px;">
        <iframe
          src="https://www.xrptipbot.com/donate/button?to=rippleitinNZ&amp;network=twitter&amp;amount=1&amp;label=null&amp;labelpt=null&amp;unique=null&amp;redirect=null&amp;stylesheet=null"
          width="240"
          height="60"
          class="xrptipbot-tipper"
          marginheight="0"
          align="top"
          scrolling="no"
          frameborder="0"
          style="width: 240px; height: 60px; border: none;"
        ></iframe>
      </div>

      <div>&#169; rippleitin.nz 2019 - new zealand's original XRP validating node</div>
      <div class="col"></div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

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
  components: { VueApexCharts },
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
      xrpMetricsDay: {},

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