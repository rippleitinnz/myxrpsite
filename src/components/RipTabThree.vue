<template>
  <div
    class="cardouter tab-pane h-100 p-3 rounded"
    id="rippleitinnz"
    role="tabpanel"
    aria-labelledby="rippleitinnz-tab"
  >
    <div class="card" style="width: 885px;margin: 15px;">
      <div id="accordian2">
        <!-- card vote-->

        <div class="card-header" id="heading12" align="left">
          <h5 class="mb-0">
            <button
              class="btn btn-link collapsed"
              data-toggle="collapse"
              data-target="#collapse12"
              aria-expanded="false"
              aria-controls="collapse12"
            >Rippleitin Vote History</button>
            <button
              class="btn btn-link collapsed float-right"
              data-toggle="collapse"
              data-target="#collapse12"
              aria-expanded="false"
              aria-controls="collapse12"
            >
              <div class="picright">
                <img class="xrpJPG" />
              </div>
            </button>
          </h5>
        </div>

        <div id="collapse12" class="collapse" aria-labelledby="heading12" data-parent="#accordian2">
          <div class="card-body">
            <div class="row">
              <div
                class="table-responsive"
                style="margin-left: -6px; margin-right: -6px;margin-top: -20px; width: 885px;"
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
                      <th class="text-left" scope="col">Our Vote</th>

                      <th class="text-left" scope="col">&nbsp;</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="bash in Object.keys(featureStates)" v-bind:key="bash">
                      <td class="text-left" scope="col">&nbsp;</td>
                      <td
                        class="text-left"
                        :class="{ 'orange': featureStates[bash].count}"
                      >{{ featureStates[bash].name }}</td>
                      <td
                        class="text-left"
                        :class="{ 'orange': featureStates[bash].count}"
                      >{{ featureStates[bash].enabled ? 'Enabled' : 'Disabled' }}</td>
                      <td class="text-left" scope="col">&nbsp;</td>

                      <td class="text-left" :class="{ 'blue': featureStates[bash].count }">
                        <div v-if="!featureStates[bash].count"></div>
                        <div
                          v-else
                        >{{ featureStates[bash].count }}/{{ featureStates[bash].threshold }}</div>
                      </td>

                      <td class="text-left" scope="col">&nbsp;</td>
                      <td
                        class="text-left"
                        :class="{ 'red': featureStates[bash].vetoed }"
                      >{{ featureStates[bash].vetoed ? 'Vetoed' : 'Supported' }}</td>

                      <td class="text-left" scope="col">&nbsp;</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>

        <!--vote -->
      </div>
    </div>
  </div>

  <!--Tab 3 No Cards-->
</template>

<script>
export default {
  data: function() {
    return {
      //Rawdata from my validator
      serverStates: null, //rippleitinnz server
      featureStates: null //rippleitinz voting
    };
  },
  //end of data

  mounted() {
    // Connect to server status
    this.serverStateVal();
  },

  methods: {
    serverStateVal() {
      const endpoint =
        process.env.NODE_ENV === "development"
          ? "ws://192.168.1.50:8181"
          : "wss://rippleitin.nz";
      const sk = new WebSocket(endpoint);
      sk.onmessage = mk => {
        mk.data;
        const parsedJson = JSON.parse(mk.data);
        
        if (typeof parsedJson.build_version === "undefined") {
          this.featureStates = parsedJson;
          console.log(parsedJson);
          
        } else {
          this.serverStates = parsedJson;
        }
      };
    }
  }

  //end of methods
};
</script>

