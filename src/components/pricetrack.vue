<template>
<!-- vue root -->
<div id="apparatus" v-cloak>



<!-- price list grid -->
<main class="main-wrap">
  <div class="main-grid-list">
    <div class="main-grid-item" v-for="c in coinsList" :key="c.symbol" :class="c.style">
      <div class="main-grid-info flex-row flex-top flex-stretch">
        <div class="push-right">
          <img :src="c.icon" :alt="c.pair" />
        </div>
        <div class="flex-1 shadow-text">
          <div class="flex-row flex-top flex-space">
            <div class="text-left text-clip push-right">
              <h1 class="text-primary text-clip">{{ c.token }}<small class="text-faded text-small text-condense">/{{ c.asset }}</small></h1>
              <h2 class="text-bright text-clip">{{ c.close | toFixed( asset ) }}</h2>
            </div>
            <div class="text-right">
              <div class="color text-big text-clip">{{ c.arrow }} {{ c.sign }}{{ c.percent | toFixed( 2 ) }}%</div>
              <div class="text-clip">{{ c.sign }}{{ c.change | toFixed( asset ) }} <small class="text-faded">24h</small></div>
              <div class="text-clip">{{ c.assetVolume | toMoney }} <small class="text-faded">Vol</small></div>
            </div>
          </div>
        </div>
      </div>
      <div class="main-grid-chart">
        <linechart :width="600" :height="40" :values="c.history"></linechart>
      </div>
    </div>
  </div>
</main>

<!-- socket loader -->
<div class="loader-wrap" :class="{ 'visible': loaderVisible }">
  <div class="loader-content">
    <div v-if="status === 0"><i>📡</i> <br /> Connecting to Socket API ...</div>
    <div v-else-if="status === 1"><i>💬</i> <br /> Waiting for data from Socket API ...</div>
    <div v-else-if="status === 2"><i>😃</i> <br /> Connected to the Socket API</div>
    <div v-else-if="status === -1"><i>😡</i> <br /> Error connecting to the Socket API</div>
  </div>
</div>

</div>

</template>

<script>
// common number filters
Vue.filter( 'toFixed', ( num, asset ) => {
  if ( typeof asset === 'number' ) return Number( num ).toFixed( asset );
  return Number( num ).toFixed( ( asset === 'USDT' ) ? 3 : 8 );
});
Vue.filter( 'toMoney', num => {
  return Number( num ).toFixed( 0 ).replace( /./g, ( c, i, a ) => {
    return i && c !== "." && ( ( a.length - i ) % 3 === 0 ) ? ',' + c : c;
  });
});

// component for creating line chart
Vue.component( 'linechart', {
  props: {
    width: { type: Number, default: 400, required: true },
    height: { type: Number, default: 40, required: true },
    values: { type: Array, default: [], required: true },
  },
  data() {
    return { cx: 0, cy: 0 };
  },
  computed: {
    viewBox() {
      return '0 0 '+ this.width +' '+ this.height;
    },
    chartPoints() {
      let data = this.getPoints();
      let last = data.length ? data[ data.length - 1 ] : { x: 0, y: 0 };
      let list = data.map( d => ( d.x - 10 ) +','+ d.y );
      this.cx  = last.x - 5;
      this.cy  = last.y;
      return list.join( ' ' );
    },
  },
  methods: {
    getPoints() {
      this.width  = parseFloat( this.width ) || 0;
      this.height = parseFloat( this.height ) || 0;
      let min     = this.values.reduce( ( min, val ) => val < min ? val : min, this.values[ 0 ] );
      let max     = this.values.reduce( ( max, val ) => val > max ? val : max, this.values[ 0 ] );
      let len     = this.values.length;
      let half    = this.height / 2;
      let range   = ( max > min ) ? ( max - min ) : this.height;
      let gap     = ( len > 1 ) ? ( this.width / ( len - 1 ) ) : 1;
      let points  = [];

      for ( let i = 0; i < len; ++i ) {
        let d = this.values[ i ];
        let val = 2 * ( ( d - min ) / range - 0.5 );
        let x = i * gap;
        let y = -val * half * 0.8 + half;
        points.push( { x, y } );
      }
      return points;
    }
  },
  template: `
  <svg :viewBox="viewBox" xmlns="http://www.w3.org/2000/svg">
    <polyline class="color" fill="none" stroke="#fff" stroke-width="2" stroke-linecap="round" :points="chartPoints" />
    <circle class="color" :cx="cx" :cy="cy" r="4" fill="#fff" stroke="none" />
  </svg>`,
});

// vue instance
new Vue({
  // mount point
  el: '#apparatus',

  // apparatus data
  data: {
    endpoint : 'wss://stream.binance.com:9443/ws/!ticker@arr',
    iconbase : 'https://raw.githubusercontent.com/rainner/binance-watch/master/public/images/icons/',
    cache    : {},             // coins data cache
    coins    : [],             // live coin list from api
    asset    : 'BTC',          // filter by base asset pair
    search   : '',             // filter by search string
    sort     : 'assetVolume',  // sort by param
    order    : 'desc',         // sort order ( asc, desc )
    limit    : 50,             // limit list
    status   : 0,              // socket status ( 0: closed, 1: open, 2: active, -1: error )
    sock     : null,           // socket inst
    cx       : 0,
    cy       : 0,
  },

  // computed methods
  computed: {

    // process coins list
    coinsList() {
      let list = this.coins.slice();
      let search = this.search.replace( /[^\s\w\-\.]+/g, '' ).replace( /[\r\s\t\n]+/g, ' ' ).trim();

      if ( this.asset ) {
        list = list.filter( i => i.asset === this.asset );
      }
      if ( search && search.length > 1 ) {
        let reg = new RegExp( '^('+ search +')', 'i' );
        list = list.filter( i => reg.test( i.token ) );
      }
      if ( this.sort ) {
        list = this.sortList( list, this.sort, this.order );
      }
      if ( this.limit ) {
        list = list.slice( 0, this.limit );
      }
      return list;
    },

    // show socket connection loader
    loaderVisible() {
      return ( this.status === 2 ) ? false : true;
    },

    // sort-by label for buttons, etc
    sortLabel() {
      switch ( this.sort ) {
        case 'token'       :  return 'Token';
        case 'percent'     :  return 'Percent';
        case 'close'       :  return 'Price';
        case 'change'      :  return 'Change';
        case 'assetVolume' :  return 'Volume';
        case 'tokenVolume' :  return 'Volume';
        case 'trades'      :  return 'Trades';
        default            :  return 'Default';
      }
    },
  },

  // custom methods
  methods: {

    // apply sorting and toggle order
    sortBy( key, order ) {
      if ( this.sort !== key ) { this.order = order || 'asc'; }
      else { this.order = ( this.order === 'asc' ) ? 'desc' : 'asc'; }
      this.sort = key;
    },

    // filter by asset
    filterAsset( asset ) {
      this.asset = String( asset || 'BTC' );
    },

    // set list limit
    setLimit( limit ) {
      this.limit = parseInt( limit ) || 0;
    },

    // on socket connected
    onSockOpen( e ) {
      this.status = 1; // open
      console.info( 'WebSocketInfo:', 'Connection open ('+ this.endpoint +').' );
    },

    // on socket closed
    onSockClose( e ) {
      this.status = 0; // closed
      console.info( 'WebSocketInfo:', 'Connection closed ('+ this.endpoint +').' );
      setTimeout( this.sockInit, 10000 ); // try again
    },

    // on socket error
    onSockError( err ) {
      this.status = -1; // error
      console.error( 'WebSocketError:', err.message || err );
      setTimeout( this.sockInit, 10000 ); // try again
    },

    // process data from socket
    onSockData( e ) {
      let list = JSON.parse( e.data ) || [];

      for ( let item of list ) {
        // cleanup data for each coin
        let c = this.getCoinData( item );
        // keep to up 100 previous close prices in hostiry for each coin
        c.history = this.cache.hasOwnProperty( c.symbol ) ? this.cache[ c.symbol ].history : this.fakeHistory( c.close );
        if ( c.history.length > 100 ) c.history = c.history.slice( c.history.length - 100 );
        c.history.push( c.close );
        // add coin data to cache
        this.cache[ c.symbol ] = c;
      }
      // convert cache object to final prices list for each symbol
      this.coins = Object.keys( this.cache ).map( s => this.cache[ s ] );
      this.status = 2; // active
    },

    // start socket connection
    sockInit() {
      if ( this.status > 0 ) return;
      try {
        this.status = 0; // closed
        this.sock = new WebSocket( this.endpoint );
        this.sock.addEventListener( 'open', this.onSockOpen );
        this.sock.addEventListener( 'close', this.onSockClose );
        this.sock.addEventListener( 'error', this.onSockError );
        this.sock.addEventListener( 'message', this.onSockData );
      }
      catch( err ) {
        console.error( 'WebSocketError:', err.message || err );
        this.status = -1; // error
        this.sock = null;
      }
    },

    // start socket connection
    sockClose() {
      if ( this.sock ) {
        this.sock.close();
      }
    },

    // come up with some fake history prices to fill in the initial line chart
    fakeHistory( close ) {
      let num = close * 0.0001; // faction of current price
      let min = -Math.abs( num );
      let max = Math.abs( num );
      let out = [];

      for ( let i = 0; i < 50; ++i ) {
        let rand = Math.random() * ( max - min ) + min;
        out.push( close + rand );
      }
      return out;
    },

    // finalize data for each coin from socket
    getCoinData( item ) {
      let reg         = /^([A-Z]+)(BTC|ETH|BNB|USDT|TUSD)$/;
      let symbol      = String( item.s ).replace( /[^\w\-]+/g, '' ).toUpperCase();
      let token       = symbol.replace( reg, '$1' );
      let asset       = symbol.replace( reg, '$2' );
      let name        = token;
      let pair        = token +'/'+ asset;
      let icon        = this.iconbase + token.toLowerCase() + '_.png';
      let open        = parseFloat( item.o );
      let high        = parseFloat( item.h );
      let low         = parseFloat( item.l );
      let close       = parseFloat( item.c );
      let change      = parseFloat( item.p );
      let percent     = parseFloat( item.P );
      let trades      = parseInt( item.n );
      let tokenVolume = Math.round( item.v );
      let assetVolume = Math.round( item.q );
      let sign        = ( percent >= 0 ) ? '+' : '';
      let arrow       = ( percent >= 0 ) ? '▲' : '▼';
      let info        = [ pair, close.toFixed( 8 ), '(', arrow, sign + percent.toFixed( 2 ) +'%', '|', sign + change.toFixed( 8 ), ')' ].join( ' ' );
      let style       = '';

      if ( percent > 0 ) style = 'gain';
      if ( percent < 0 ) style = 'loss';

      return { symbol, token, asset, name, pair, icon, open, high, low, close, change, percent, trades, tokenVolume, assetVolume, sign, arrow, style, info };
    },

    // sort an array by key and order
    sortList( list, key, order ) {
      return list.sort( ( a, b ) => {
        let _a = a[ key ];
        let _b = b[ key ];

        if ( _a && _b ) {
          _a = ( typeof _a === 'string' ) ? _a.toUpperCase() : _a;
          _b = ( typeof _b === 'string' ) ? _b.toUpperCase() : _b;

          if ( order === 'asc' ) {
            if ( _a < _b ) return -1;
            if ( _a > _b ) return 1;
          }
          if ( order === 'desc' ) {
            if ( _a > _b ) return -1;
            if ( _a < _b ) return 1;
          }
        }
        return 0;
      });
    },
  },

  // apparatus mounted
  mounted() {
    this.sockInit();
  },

  // apparatus destroyed
  destroyed() {
    this.sockClose();
  }
});

</script>

<style lang="scss" scoped>


// spacing and padding
$padSpace: 1em;
$padSmall: $padSpace / 2;
$headerHeight: 4em;

// borders and lines
$lineWidth: 2px;
$lineStyle: solid;
$lineColor: rgba( 255, 255, 255, 0.04 );
$lineJoin: 3px;

// document colors
$colorDocument: #0c0d0f;
$colorDocumentText: desaturate( lighten( $colorDocument, 50% ), 20% );
$colorDocumentLight: lighten( $colorDocument, 8% );
$colorDocumentDark: darken( $colorDocument, 6% );

// common colors
$colorDefault: #828a98;
$colorDefaultText: #dae5ed;
$colorPrimary: orange;
$colorPrimaryText: darken( $colorPrimary, 40% );
$colorSecondary: #20acea;
$colorSecondaryText: darken( $colorSecondary, 40% );
$colorGrey: #5c6776;
$colorGreyText: lighten( $colorGrey, 40% );
$colorBright: #f0f0f0;
$colorBrightText: #1f2833;
$colorGain: limegreen;
$colorGainText: lighten( $colorGain, 40% );
$colorLoss: crimson;
$colorLossText: lighten( $colorLoss, 40% );

// base font options
$fontSize: 15px;
$fontSpace: 1.2em;
$fontWeight: 600;
$fontFamily: monospace;

// shadow styles
$shadowPaper: 0 1px 2px rgba( 0, 0, 0, 0.3 );
$shadowDark: 0 1px 8px rgba( 0, 0, 0, 0.6 );
$shadowGlow: 0 0 10px rgba( 0, 0, 0, 0.2 );

// transition props
$fxSpeed: 300ms;
$fxEase: cubic-bezier( 0.215, 0.610, 0.355, 1.000 );

// screen sizes
$sizeSmall: 420px;
$sizeMedium: 720px;
$sizeLarge: 1200px;

// screen breakpoints
$screenSmall: "only screen and (min-width : #{$sizeSmall})";
$screenMedium: "only screen and (min-width : #{$sizeMedium})";
$screenLarge: "only screen and (min-width : #{$sizeLarge})";

// page reset
*, *:before, *:after {
  margin: 0;
  padding: 0;
  border: 0;
  outline: none;
  background-color: transparent;
  text-transform: none;
  text-shadow: none;
  box-shadow: none;
  box-sizing: border-box;
  appearance: none;
  -webkit-overflow-scrolling: touch;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  transform-style: flat;
  transition:
    border-color $fxSpeed $fxEase,
    background-color $fxSpeed $fxEase,
    opacity $fxSpeed $fxEase,
    transform $fxSpeed $fxEase;
}

// block types
article, aside, details, figcaption, figure, footer, header, hgroup,
menu, nav, section, main, summary, div, h1, h2, h3, h4, h5, h6, hr,
p, ol, ul, form, img {
  display: block;
}

// document setup
html, body {
  display: block;
  position: relative;
  max-width: 100vw;
  min-height: 100vh;
}
html {
  overflow: hidden;
  overflow-y: scroll;
}
body {
  font-family: $fontFamily;
  font-weight: $fontWeight;
  font-size: calc( #{$fontSize} - 6px );
  line-height: $fontSpace;
  background-color: $colorDocument;
  color: $colorDocumentText;

  @media #{$screenSmall} {
    font-size: calc( #{$fontSize} - 4px );
  }
  @media #{$screenMedium} {
    font-size: calc( #{$fontSize} - 2px );
  }
  @media #{$screenLarge} {
    font-size: $fontSize;
  }
}

// headings
h1, h2, h3, h4, h5, h6 {
  display: block;
  font-weight: inherit;
  line-height: $fontSpace;
}

// horizontal lines
hr {
  display: block;
  overflow: hidden;
  margin: $padSpace 0;
  height: 0;
  border: 0;
  border-bottom: $lineWidth $lineStyle $lineColor;
}

// inputs
input, button, select, option, textarea {
  display: block;
  font-family: inherit;
  font-size: inherit;
  line-height: inherit;
  color: inherit;
}

// media query helpers
.if-small {
  display: none;
  @media #{$screenSmall} {
    display: initial;
  }
}
.if-medium {
  display: none;
  @media #{$screenMedium} {
    display: initial;
  }
}
.if-large {
  display: none;
  @media #{$screenLarge} {
    display: initial;
  }
}

// not rendered
.hidden, [hidden], [v-cloak] {
  display: none;
}

// visible but not usable
.disabled, [disabled] {
  pointer-events: none;
  opacity: 0.5;
}

// common card style
.card {
  padding: $padSpace;
  background-color: $colorDocumentLight;
  border-radius: $lineJoin;
  box-shadow: $shadowPaper;
}

// margin helpers
.push-top { margin-top: $padSpace; }
.push-right { margin-right: $padSpace; }
.push-bottom { margin-bottom: $padSpace; }
.push-left { margin-left: $padSpace; }
.push-all { margin: $padSpace; }

// padding helpers
.pad-top { padding-top: $padSpace; }
.pad-right { padding-right: $padSpace; }
.pad-bottom { padding-bottom: $padSpace; }
.pad-left { padding-left: $padSpace; }
.pad-all { padding: $padSpace; }

// border helpers
.border-top { border-top: $lineWidth $lineStyle $lineColor; }
.border-right { border-right: $lineWidth $lineStyle $lineColor; }
.border-bottom { border-bottom: $lineWidth $lineStyle $lineColor; }
.border-left { border-left: $lineWidth $lineStyle $lineColor; }

// flex helpers
.flex-row { display: flex; flex-direction: row; flex-wrap: nowrap; }
.flex-wrap { flex-wrap: wrap; }
.flex-left { justify-content: flex-start; }
.flex-center { justify-content: center; }
.flex-right { justify-content: flex-end; }
.flex-space { justify-content: space-between; }
.flex-around { justify-content: space-around; }
.flex-top { align-items: flex-start; }
.flex-middle { align-items: center; }
.flex-bottom { align-items: flex-end; }
.flex-1 { flex: 1; }
.flex-2 { flex: 2; }
.flex-3 { flex: 3; }
.flex-4 { flex: 4; }
.flex-5 { flex: 5; }

// text helpers
.text-left { text-align: left; }
.text-right { text-align: right; }
.text-center { text-align: center; }
.text-justify { text-align: justify; }
.text-uppercase { text-transform: uppercase; }
.text-lowercase { text-transform: lowercase; }
.text-capitalize { text-transform: capitalize; }
.text-underline { text-decoration: underline; }
.text-striked { text-decoration: line-through; }
.text-italic { font-style: italic; }
.text-bold { font-weight: bold; }
.text-nowrap { white-space: nowrap; }
.text-clip { overflow: hidden; white-space: nowrap; text-overflow: ellipsis; }
.text-primary { color: $colorPrimary; }
.text-secondary { color: $colorSecondary; }
.text-grey { color: $colorGrey; }
.text-bright { color: $colorBright; }
.text-faded { color: white; opacity: 0.3; }
.text-big { font-size: 120%; line-height: ( $fontSpace * 1.01 ); }
.text-small { font-size: 70%; line-height: ( $fontSpace * 0.95 ); }
.text-condense { letter-spacing: -1px; }

// shadow helpers
.shadow-box { box-shadow: $shadowPaper; }
.shadow-text { text-shadow: $shadowPaper; }

// input wrapper
.form-input {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  align-items: center;
  padding: ( $padSpace * .7 ) $padSpace;
  color: $colorBright;
  background-color: lighten( $colorDocument, 20% );
  border-radius: 100px;

  &.dark {
    background-color: darken( $colorDocument, 20% );
  }
  & > input {
    width: auto;
  }
}

// menu dropdown
@keyframes dropdownShow {
    0% { transform: translateY( 30px ); opacity: 0; }
  100% { transform: translateY( 0 ); opacity: 1; }
}
.dropdown {
  display: block;
  position: relative;
  cursor: pointer;

  & > ul {
    display: none;
    list-style: none;
    position: absolute;
    transition: none;
    animation: dropdownShow $fxSpeed $fxEase forwards;
    right: 0;
    top: 50%;
    min-width: 200px;
    max-width: 400px;
    padding: ( $padSpace / 2 ) 0;
    background-color: lighten( $colorDocumentLight, 4% );
    border-radius: $lineJoin;
    box-shadow: $shadowDark;

    & > li {
      display: block;
      padding: ( $padSpace / 2 ) $padSpace;
      background-color: rgba( #000, 0 );
      color: $colorBright;
      cursor: pointer;

      & + li {
        border-top: $lineWidth $lineStyle $lineColor;
      }
      &:hover {
        background-color: rgba( #000, 0.1 );
      }
    }
  }

  &:hover > ul,
  &:active > ul {
    display: block;
  }
}

// apparatus header
.header-wrap {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  background-color: $colorDocumentLight;
  background-image: radial-gradient( ellipse at top, rgba( #fff, 0.1 ) 0%, transparent 60% );
  box-shadow: $shadowDark;
  z-index: 999;

  .header-row {
    height: $headerHeight;
    padding: $padSpace;

    .dropdown {
      margin-left: .4em;
    }
  }
}

// price grid
.main-wrap {
  position: relative;
  padding: calc( #{$headerHeight} + #{$padSpace} ) $padSpace $padSpace $padSpace;

  .main-grid-list {
    display: grid;
    grid-template-columns: repeat( auto-fill, minmax( 200px, 1fr ) );
    grid-gap: $padSpace;

    @media #{$screenSmall} {
      grid-template-columns: repeat( auto-fill, minmax( 380px, 1fr ) );
    }

    .main-grid-item {
      background-color: $colorDocumentLight;
      background-image: linear-gradient( 65deg, rgba( #fff, 0.02 ) 40%, transparent 40% );
      border-radius: $lineJoin;
      box-shadow: $shadowPaper;

      &.gain {
        background-color: desaturate( darken( $colorGain, 35% ), 50% );
        polyline.color { stroke: $colorGain; }
        circle.color { fill: $colorGain; }
        .color { color: $colorGain; }
      }
      &.loss {
        background-color: desaturate( darken( $colorLoss, 32% ), 50% );
        polyline.color { stroke: $colorLoss; }
        circle.color { fill: $colorLoss; }
        .color { color: $colorLoss; }
      }

      .main-grid-info {
        padding: $padSpace;

        img {
          width: auto;
          height: 42px;

          @media #{$screenSmall} {
            height: 52px;
          }
          @media #{$screenMedium} {
            height: 64px;
          }
        }
      }

      .main-grid-chart {
        padding: $padSpace;
        background-image: radial-gradient( ellipse at top right, rgba( #000, 0.4 ) 0%, rgba( #000, 0 ) 60% );
      }
    }
  }
}

// loader
.loader-wrap {
  display: none;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba( #000, 0.8 );
  text-align: center;
  z-index: 9999;

  &.visible {
    display: flex;
  }

  .loader-content {
    padding: $padSpace ( $padSpace * 2 );
    background-color: $colorDocumentLight;
    border-radius: $lineJoin;
    box-shadow: $shadowPaper;

    i {
      font-style: normal;
      font-size: 600%;
      line-height: normal;
    }
  }
}

</style>

