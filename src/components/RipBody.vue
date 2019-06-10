<template>
  <div id="app">
    <div>
      <div class="margtop">
        <div class="container-fluid" align="center" style="width: 950px; margin-bottom: 60px;">
          <RipTabsMake/>

          <div class="tab-content h-75">
            <!-- Tab Components -->

            <RipTabOne/>
            <RipTabTwo/>
            <RipTabThree/>
            <RipTabFour/>

            <!-- Tab Componenets End -->
          </div>
        </div>
      </div>
    </div>
    <RipCharts/>
  </div>
</template>

<script>
import axios from "axios";
import RipTabsMake from "./RipTabsMake";
import RipTabOne from "./RipTabOne";
import RipTabTwo from "./RipTabTwo";
import RipTabThree from "./RipTabThree";
import RipTabFour from "./RipTabFour";
import RipCharts from "./RipCharts";

export default {
  name: "XRP",

  components: {
    RipTabsMake,
    RipTabOne,
    RipTabTwo,
    RipTabThree,
    RipTabFour,
    RipCharts
  },
  //----------------------------------
  data: function() {
    return {};
  } //end of return
};
</script>






