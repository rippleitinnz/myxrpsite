<template>
  <!-- Tab One-->

  <div
    class="cardouter tab-pane h-100 p-3 rounded"
    id="XRPL"
    role="tabpanel"
    aria-labelledby="XRPL-tab"
  >
    <div class="card" style="width: 885px; margin: 15px; ">
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
                <img class="xrpJPG" />
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
                      <tr scope="row" v-for="p in xrpMetrics.stats" :key="`idp-${p.date}`">
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
                <img class="xrpJPG" />
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
                      <tr scope="row" v-for="rw in $parent.xrpMetricsAll.stats" :key="`idk-${rw.date}`">
                        <td></td>
                        <td
                          class="text-left"
                        >{{ new Date(Date.parse(rw.date)).toLocaleDateString('en-NZ',{ month: 'long', day: 'numeric', year: 'numeric'}).replace(',',' ') }}</td>
                        <td class="text-right">{{ rw.metric.ledger_count.toLocaleString() }}</td>
                        <td class="text-right">{{ (rw.metric.ledger_interval * 1).toFixed(5)}}</td>
                        <td class="text-right">{{ rw.metric.transaction_count.toLocaleString() }}</td>
                        <td class="text-right">{{ (rw.metric.tx_per_ledger * 1).toFixed(5)}}</td>
                        <td
                          class="text-right"
                        >{{ rw.metric.accounts_created.toLocaleString() }} &nbsp; &nbsp;</td>
                        <td class="text-right">{{ sumCreated(rw).toLocaleString() }}&nbsp; &nbsp;</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- card vote-->

        <div class="card-header" id="heading16" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapse16"
              aria-expanded="false"
              aria-controls="collapse16"
            >XRP Ledger Amendment Status</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapse16"
              aria-expanded="false"
              aria-controls="collapse16"
            >
              <div class="picright">
                <img class="xrpJPG" />
              </div>
            </button>
          </h5>
        </div>

        <div id="collapse16" class="collapse" aria-labelledby="heading16" data-parent="#accordian">
          <div class="card-body">
            <div class="row">
              <div
                class="table-responsive"
                style="margin-left: -6px; margin-top: -20px; width: 885px;"
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
                      <th class="text-left" scope="col">Our Validator's Vote</th>

                      <th class="text-left" scope="col">&nbsp;</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="hash in Object.keys(featureState)" :key="`idm-${hash}`">
                      <td class="text-left" scope="col">&nbsp;</td>
                      <td
                        class="text-left"
                        :class="{ 'orange': featureState[hash].count}"
                      >{{ featureState[hash].name }}</td>
                      <td
                        class="text-left"
                        :class="{ 'orange': featureState[hash].count}"
                      >{{ featureState[hash].enabled ? 'Enabled' : 'Disabled' }}</td>
                      <td class="text-left" scope="col">&nbsp;</td>

                      <td class="text-left" :class="{ 'blue': featureState[hash].count }">
                        <div v-if="!featureState[hash].count"></div>
                        <div
                          v-else
                        >{{ featureState[hash].count }}/{{ featureState[hash].threshold }}</div>
                      </td>

                      <td class="text-left" scope="col">&nbsp;</td>
                      <td
                        class="text-left"
                        :class="{ 'red': featureState[hash].vetoed }"
                      >{{ featureState[hash].vetoed ? 'Vetoed' : 'Supported' }}</td>

                      <td class="text-left" scope="col">&nbsp;</td>
                    </tr>
                  </tbody>
                </table>
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
            >XRP Ledger Validators - ALL</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapseFour"
              aria-expanded="false"
              aria-controls="collapseFour"
            >
              <div class="picright">
                <img class="xrpJPG" />
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
                      <tr scope="row" v-for="q in validatorsData.validators" :key="`ijk-${q.validation_public_key}`">
                        <td class="Tab4">
                          <a
                            :href="'https://livenet.xrpl.org/validators/' + q.validation_public_key"
                            target="_blank"
                            class="pubkeyval"
                          >&nbsp;&nbsp;{{ q.validation_public_key }}</a>
                        </td>
                        <td class="Tab4">
                          <p style="margin-bottom: 0;" v-if="q.domain"></p>
                          <p class="verified" v-else>not yet verified</p>

                          <a
                            v-if="typeof q.domain !== 'undefined'"
                            v-bind:href="'http://' + q.domain"
                            target="_blank"
                            class="lightblue"
                          >{{ q.domain }}</a>
                        </td>
                        <td class="Tab4">
                          <p class="unlColor" v-if="q.unl">yes</p>
                        </td>
                        <td class="text-left" style="margin-bottom: 0; color: darkred;">
                          <p
                            style="margin-bottom: 0;"
                            v-if="q.agreement_24h.score<1"
                          >{{ (q.agreement_24h.score * 100).toFixed(2) }}%</p>
                          <p style="margin-bottom: 0; color: green;" v-else>100%</p>
                        </td>
                        <td
                          class="text-center"
                          style="margin-bottom: 0; "
                        >{{ q.agreement_24h.total }}</td>
                        <td class="text-center" style="margin-bottom: 0; color: darkred;">
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
                <img class="xrpJPG" />
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
                  class="topTable table-responsive"
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
                        class="lightblue"
                        scope="row"
                        v-for="k in xrpBurn.rows.slice().reverse()"
                        :key="`ide-${k.date}`"
                      >
                        <td>{{ new Date(Date.parse(k.date)).toLocaleDateString('en-NZ',{ weekday: 'long', month: 'long', day: 'numeric', year: 'numeric'}).replace(',',' ') }}</td>
                        <td></td>
                        <td class="text-center">{{ (100000000000 - k.total).toLocaleString() }}</td>
                        <td></td>
                        <td class="text-center">{{ (k.total *1).toLocaleString() }}</td>
                        <td></td>
                        <td
                          class="text-center"
                        >{{ (((100000000000 - k.total) / 100000000000)*100).toFixed(4) }}%</td>
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
                  class="bottomTable table-responsive"
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
                      <tr scope="row" v-for="dd in xrpFees.rows" :key="`idf-${dd.date}`">
                        <td></td>
                        <td
                          class="text-left"
                        >{{ new Date(Date.parse(dd.date)).toLocaleDateString('en-NZ',{ month: 'long', day: 'numeric', year: 'numeric'}).replace(',',' ') }}</td>
                        <td class="text-right">{{ dd.tx_count.toLocaleString() }}</td>
                        <td class="text-right">{{ dd.min }}</td>
                        <td class="text-right" :class="{ 'blink_me': (dd.max>20) }">{{ dd.max }}</td>
                        <td class="text-right">{{ dd.avg }}</td>
                        <td class="text-right">{{ dd.total }} &nbsp; &nbsp;</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!--Card FeesMax -->
        <div class="card-header" id="headingFeesMax" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapseFeesMax"
              aria-expanded="false"
              aria-controls="collapseFeesMax"
            >Largest Fees Paid</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapseFeesMax"
              aria-expanded="false"
              aria-controls="collapseFeesMax"
            >
              <div class="picright">
                <img class="xrpJPG" />
              </div>
            </button>
          </h5>
        </div>

        <div
          id="collapseFeesMax"
          class="collapse"
          aria-labelledby="headingFeesMax"
          data-parent="#accordian"
        >
          <div class="card-body">
            <div class="row">
              <div v-if="!filteredXrpFeesMaxRows">Loading...</div>
              <!--Changed this from xrpFeesMax.rows due to methods script for >20 and sorting -->

              <div v-else>
                <div
                  class="bottomTable table-responsive"
                  style="margin-left: -6px; margin-top: -20px; width: 885px;"
                >
                  <table class="table table-striped">
                    <thead>
                      <tr class="colorrow2" style="width: 885px;">
                        <th class="text-left" scope="col">&nbsp;</th>
                        <th class="text-left" scope="col">Date</th>
                        <th class="text-left" scope="col">&nbsp;</th>
                        <th
                          class="text-middle"
                          scope="col"
                        >Highest xrp fees paid for a single transaction on the XRPL</th>
                        <th class="text-left" scope="col">&nbsp;</th>
                        <th class="text-left" scope="col">Fee Paid</th>
                        <th class="text-left" scope="col">&nbsp;</th>
                        <th class="text-left" scope="col">&nbsp;</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr scope="row" v-for="ef in filteredXrpFeesMaxRows(20)" :key="`idg-${ef.date}`">
                        <!--Changed this from xrpFeesMax.rows due to methods script for >20 and sorting -->
                        <td></td>
                        <td
                          class="text-left"
                        >{{ new Date(Date.parse(ef.date)).toLocaleDateString('en-NZ',{ month: 'long', day: 'numeric', year: 'numeric'}).replace(',',' ') }}</td>
                        <td></td>
                        <td></td>
                        <td></td>
                        <td class="text-left">x&nbsp;{{ ef.max }}</td>
                        <td></td>
                        <td></td>
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
                <img class="xrpJPG" />
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
                  class="topTable table-responsive"
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
                      <tr class="lightblue" scope="row">
                        <td />
                        <td>
                          {{ new Date(Date.parse(xrpData.data.date)).toLocaleDateString('en-NZ',{ weekday: 'long', month:
                          'long', day: 'numeric', year: 'numeric', hour: '2-digit', minute: '2-digit', second: '2-digit', timeZoneName: 'short'}).replace(',',' ') }}
                        </td>
                        <td />
                        <td>Last 24 hours</td>
                        <td />
                        <td>{{ ((xrpData.data.total* 1).toFixed(0)*1).toLocaleString() }} xrp</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
                <div
                  class="bottomTable table-responsive"
                  style="margin-left: -6px; margin-top: -20px; width: 885px;"
                >
                  <table class="table table-striped">
                    <thead>
                      <tr class="colorrow2" style="width: 885px;">
                        <th />
                        <th class="text-left" scope="col">Exchange</th>
                        <th />
                        <th class="text-right" scope="col">Volume</th>
                        <th />
                        <th />
                        <th class="text-right" scope="col">Trades</th>
                        <th />
                        <th class="text-right" scope="col">Avg Trade</th>
                        <th />
                        <th class="text-right" scope="col">Against</th>
                        <th />
                      </tr>
                    </thead>
                    <tbody>
                      <tr
                        scope="row"
                        v-for="w in xrpData.data.components.slice().sort()"
                        :key="`WWW-${w.base_volume}`"
                      >
                        <td />
                        <td class="text-left">{{ w.source }}</td>
                        <td />
                        <td
                          class="text-right"
                        >{{ (((w.base_volume * 1).toFixed(5))*1).toLocaleString() }}</td>
                        <td />
                        <td />
                        <td class="text-right">{{ w.count.toLocaleString() }}</td>
                        <td />
                        <td
                          class="text-right"
                        >{{ ((w.base_volume / w.count).toFixed(5)).toLocaleString() }}</td>
                        <td />
                        <td class="text-right">{{ w.counter_currency }}</td>
                        <td />
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
                <img class="xrpJPG" />
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
                        :key="`idz-${z.date}`"
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

   <!-- END OF CARD -->
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

      validatorsData: [],

      xrpMetrics: [],

      nodesData: [],

      xrpFees: [],

      xrpDist: [],

      xrpData: [],

      xrpBurn: [],

      xrpFeesMax: [],
      serverState: null, //rippleitinnz server
      featureState: null
    };
  },
  //--------------------------

  mounted() {
    this.serverStateHandled();
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
        "https://data.ripple.com/v2/network/fees?interval=day&descending=true&limit=365"
      )
      .then(dd => {
        return dd.json();
      })
      .then(data => {
        // Set it to the Vue App data
        this.xrpFees = data;
      });
    //rippled Lookup all max fees
    window
      .fetch(
        "https://data.ripple.com/v2/network/fees?max&descending=true&interval=day&limit=3000"
      )
      .then(ef => {
        return ef.json();
      })
      .then(data => {
        // Set it to the Vue App data
        this.xrpFeesMax = data;
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
    sumCreated(rw) {
      // No need to build a new array or total in a global,
      // we'll just lookup the current index in the stats array (indexOf)
      const currentRowIndex = this.$parent.xrpMetricsAll.stats.indexOf(rw);
      return this.$parent.xrpMetricsAll.stats
        .slice(currentRowIndex)
        .reduce((a, b) => {
          return a + b.metric.accounts_created;
        }, 0);
    },

   
    serverStateHandled() {
      const endpoint =
        process.env.NODE_ENV === "development"
          ? "ws://192.168.1.50:8181"
          : "wss://rippleitin.nz";
      const sz = new WebSocket(endpoint);
      sz.onmessage = mj => {
        const parsedJson = JSON.parse(mj.data);
        if (typeof parsedJson.build_version === "undefined") {
          this.featureState = parsedJson
        
        } else {
          this.serverState = parsedJson;
        }
      };
    },
    //filters max fees to above 20 and in highest to lowest
    filteredXrpFeesMaxRows(max) {
      //add the amount to the
      return this.xrpFeesMax.rows
        .filter(ef => ef.max)
        .sort((a, b) => {
          if (a.max > b.max) return -1;
          if (a.max < b.max) return 1;
          return 0;
        });
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
            .map(af => {
              // Chain a mapper, convert all date fields to actual JS date objects,
              // so we can call date methods in the Vue template on the date field
              return Object.assign(af, {
                date: new Date(af.date)
              });
            })
        });
      });
  }
};
</script>

 computed: {
    filteredXrpFeesMaxRows: function() {
      return this.xrpFeesMax.rows.filter(e => e.max > 20); 
          }
  }, 

