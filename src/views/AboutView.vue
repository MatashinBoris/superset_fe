<template>
  <div class="about">
    <h1>This is an about page</h1>
    <div id="superset-container"></div>
  </div>
</template>

<script>
import {embedDashboard} from "@superset-ui/embedded-sdk";

export default {

  name: 'AboutView',
  async mounted() {
    function fetchGuestTokenFromBackend() {
      return async function () {
        const response = await fetch("http://localhost:9090/api/v1/superset/auth", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify({
            "dashboardId": "b1401b55-04ed-455e-9a22-70d895855db1",
            "clientCode": "demo"
          })
        });

        if (!response.ok) {
          throw new Error("Failed to fetch the guest token from backend");
        }

        const data = await response.json();

        console.log("Token: " + data.token)
        return data.token;

        // return Promise.resolve(
        //     "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjp7InVzZXJuYW1lIjoiYm9yaXMifSwicmVzb3VyY2VzIjpbeyJ0eXBlIjoiZGFzaGJvYXJkIiwiaWQiOiJiMTQwMWI1NS0wNGVkLTQ1NWUtOWEyMi03MGQ4OTU4NTVkYjEifV0sInJsc19ydWxlcyI6W3siY2xhdXNlIjoiY2xpZW50X2NvZGU9J2RlbW8nIn1dLCJpYXQiOjE2OTgyNDE1MjIuNjc5MDk0MywiZXhwIjoxNjk4MjQxODIyLjY3OTA5NDMsImF1ZCI6Imh0dHA6Ly8wLjAuMC4wOjgwODAvIiwidHlwZSI6Imd1ZXN0In0.ajA43sL1myQOl4W9vmmSceqkFmALNTJnBDkP5qkeeZA")
      };
    }

    await embedDashboard({
      id: "b1401b55-04ed-455e-9a22-70d895855db1",
      supersetDomain: "http://localhost:8080",
      mountPoint: document.getElementById("superset-container"),
      fetchGuestToken: fetchGuestTokenFromBackend(),
      dashboardUiConfig: {
        hideTitle: true,
        filters: {
          expanded: true,
        }
      },
    });
  },
  methods: {}
}
</script>

<style>
iframe {
  position: absolute;
  left: 0;
  top: 0;
  width: 100vw;
  height: 100vh;
}

@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>