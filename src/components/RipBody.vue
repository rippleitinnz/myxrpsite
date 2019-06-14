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
            <RipTabTest/>

            <!-- Tab Componenets End -->
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
import RipTabTest from "./RipTabTest";

export default {
   components: {
    RipTabsMake,
    RipTabOne,
    RipTabTwo,
    RipTabThree,
    RipTabFour,
    RipTabTest
  },
  
  data: function() {
    return {};
  } //end of return
};
</script>






