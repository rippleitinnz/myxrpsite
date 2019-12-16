<template>
  <div
    class="tab-pane h-100 p-3 border border-info rounded"
    id="TOML"
    role="tabpanel"
    aria-labelledby="TOML-tab"
    style="background-image: linear-gradient(white 30%, rgb(98, 189, 219)400%);"
  >
    <div
      class="card"
      style="width: 885px;margin: 15px;border: 0.3px solid #5bc0de ;border-radius: 6px;"
    >
      <div class="row">
        <div
          class="text-left"
          style="font-size: 12px; margin-top: 20px; margin-left: 50px; margin-right:50px;"
        >
          <a
            class="ripple"
            href="https://rippleitin.nz/.well-known/xrp-ledger.toml"
            target="_blank"
            style="text-decoration:none;"
            title="rippleitin.nz's Rippled validator TOML File"
          >SERVER TOML FILE</a>
          <br />
          <br />
          <p>
            <br />
            <br />TOML File Contents
            <br />
            <br />[METADATA]
            <br />modified = 2019-06-16T00:00:00.000Z
            <br />
            <br />[[VALIDATORS]]
            <br />public_key = "nHUcQnmEbCNq4uhntFudzfrZV8P5WLoBrR5h3R9jAd621Aaz1pSy"
            <br />network = "main"
            <br />owner_country = "NZ"
            <br />server_country = "NZ"
            <br />unl = "https://vl.ripple.com"
            <br />
            <br />[[SERVERS]]
            <br />json_rpc = "https://rippleitin.nz: 51235/"
            <br />
desc = "General purpose server cluster"
            <br />
            <br />[[PRINCIPALS]]
            <br />name = "Chris McKay"
            <br />Company = "Outclassed Ltd"
            <br />email = "admin@rippleitin.nz"
            <br />email_other = "chris@outclassed.co.nz"
            <br />Phone = "+64 27 228 9289"
            <br />
            <br />
          </p>
        </div>
      </div>

      <br />
    </div>
  </div>
</template>
<script>
export default {};
</script>


  


