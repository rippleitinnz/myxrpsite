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
import RipCharts from './RipCharts'


export default {
  name: "XRP",

  components: { 
    RipTabOne,
    RipTabTwo,
    RipTabThree,
    RipTabFour,
    RipCharts
  
     },
  //----------------------------------
  data: function() {
    return {
      
    };
  }, //end of return
 
};
</script>






