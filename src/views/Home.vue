<template>
  <div class="home">
    <div class="banner">
      <div class="banner__content">
        <h1 class="banner__title text-white">IP Address Tracker</h1>
        <div class="search-box">
          <input
            v-model="ipQuery"
            type="text"
            class="search-input"
            placeholder="Search for any IP address or domain"
          />
          <i
            @click="getIpInfo"
            class="fa fa-solid fa-chevron-right text-white"
          ></i>
        </div>
      </div>
      <IPInfoDisplay :ipInfo="ipInfo" v-if="ipInfo" />
    </div>
    <div id="map" class="map-display"></div>
  </div>
</template>

<script>
import IPInfoDisplay from "@/components/IPInfoDisplay";
import leaflet from "leaflet";
import { onMounted, ref } from "vue";
import axios from "axios";
export default {
  name: "Home",
  components: { IPInfoDisplay },
  setup() {
    let viewMap;
    const ipQuery = ref("");
    const ipInfo = ref(null);

    onMounted(() => {
      // Set a default latitude and longitude
      viewMap = leaflet.map("map").setView([37.38605, -122.08385], 13);

      leaflet
        .tileLayer(
          "https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=pk.eyJ1IjoiemFja25lcm9uIiwiYSI6ImNsMW5wcmo5NjBxMmszZnRjNW5icnp4NncifQ.1I-oypRQmzK4jIDiRiaCQw",
          {
            maxZoom: 18,
            id: "mapbox/streets-v11",
            tileSize: 512,
            zoomOffset: -1,
            accessToken:
              "pk.eyJ1IjoiemFja25lcm9uIiwiYSI6ImNsMW5wcmo5NjBxMmszZnRjNW5icnp4NncifQ.1I-oypRQmzK4jIDiRiaCQw",
          }
        )
        .addTo(viewMap);
    });

    const getIpInfo = async () => {
      try {
        const data = await axios.get(
          `https://geo.ipify.org/api/v2/country,city?apiKey=at_rDMvmLgNGox0PYdrbSm38za9IfKOJ&ipAddress=${ipQuery.value}`
        );

        const result = data.data;
        ipInfo.value = {
          address: result.ip,
          state: result.location.region,
          timezone: result.location.timezone,
          isp: result.isp,
          lat: result.location.lat,
          lng: result.location.lng,
        };

        leaflet.marker([ipInfo.value.lat, ipInfo.value.lng]).addTo(viewMap);
        viewMap.setView([ipInfo.value.lat, ipInfo.value.lng], 13);
      } catch (error) {
        alert(error.message);
      }
    };

    return { ipQuery, ipInfo, getIpInfo };
  },
};
</script>

<style>
/* Utilities class */
.d-flex {
  display: flex;
}
.flex-col {
  flex-direction: column;
}
.flex-row {
  flex-direction: row;
}
.p-relative {
  position: relative;
}
.p-absolute {
  position: absolute;
}

.text-uppercase {
  text-transform: uppercase;
}

.text-white {
  color: #fff;
}

.bg-white {
  background-color: #fff;
}

/* Banner */

.banner {
  background: url(~@/assets/pattern-bg.png) no-repeat;
  background-size: cover;
  height: 375px;
  padding: 0 30px;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  position: relative;
}

.banner__content {
  z-index: 20;
  width: 80%;
  position: absolute;
  top: 10%;
}

.banner__title {
  font-weight: 400;
  font-size: 25px;
  margin-bottom: 20px;
}

/* ---------------------- */

.search-box {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  margin: auto;
  margin-bottom: 40px;
}

.search-input {
  outline: none;
  padding: 8px 10px;
  width: 100%;
  height: 50px;
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
  border: 0;
}

.fa {
  cursor: pointer;
  background: #000;
  padding: 17px 17px;
  border-top-right-radius: 5px;
  border-bottom-right-radius: 5px;
}

.map-display {
  height: 100vh;
  z-index: 10;
}

@media screen and (min-width: 700px) {
  .banner {
    height: 250px;
  }
  .banner__content {
    margin-top: 45px;
    position: absolute;
    top: 0;
  }
  .search-input {
    height: 34px;
  }
  .fa {
    padding: 9px 10px;
  }
  .search-box {
    width: 50%;
  }
}

@media screen and (min-width: 975px) {
  .banner__content {
    width: 45%;
  }
}
</style>
