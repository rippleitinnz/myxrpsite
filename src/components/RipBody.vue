<template>
  <div id="app">
    <div>
      <div class="margtop">
        <div class="container-fluid" align="center" style="width: 950px; margin-bottom: 60px;">
          <RipTabsMake />

          <div class="tab-content h-75">
            <!-- Tab Components -->
            <!-- <RipTabXmas /> -->
            <RipTabOne />
            <RipTabTwo />
            <RipTabThree />
            <RipTabFour />
            <RipTabFive />
            <RipTabTOML />

            <!-- Tab Components End -->
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import RipTabsMake from "./RipTabsMake";
import RipTabOne from "./RipTabOne";
import RipTabTwo from "./RipTabTwo";
import RipTabThree from "./RipTabThree";
import RipTabFour from "./RipTabFour";
import RipTabFive from "./RipTabFive";
import RipTabTOML from "./RipTabTOML";

export default {
  components: {
    RipTabsMake,
    RipTabOne,
    RipTabTwo,
    RipTabThree,
    RipTabFour,
    RipTabFive,
    RipTabTOML
  },

  data: function() {
    return {
      xrpMetricsAll: {}
    };
  } //end of return
};
</script>
