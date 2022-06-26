<template>
  <div class="flex flex-col h-screen max-h-screen">
    <!-- Search / Results -->
    <div
      class="z-20 flex justify-center relative bg-hero-pattern bg-cover px-4 pt-8 pb-32 bg-red-500"
    >
      <!-- Search Input -->
      <div class="w-full max-w-screen-sm">
        <h1 class="text-whit text-center text-3xl pb-4">IP Address Tracker</h1>
        <div class="flex">
          <input
            v-model="queryIp"
            class="flex-1 py-3 px-2 rounded-tl-md rounded-bl-md focus:outline-none"
            type="text"
            placeholder="Search for any IP Address or leave empty to get yours"
          />
          <i
            @click="getIpInfo"
            class="cursor-pointer bg-black text-whit px-4 rounded-tr-md rounded-br-md flex items-center fas fa-chevron-right"
          ></i>
        </div>
      </div>

      <!-- IP Info -->
      <IPInfo v-if="ipInfo" :ipInfo="ipInfo" />
    </div>
    <!-- map -->
    <div id="map" class="h-full z-10"></div>
  </div>
</template>

<script>
import IPInfo from "@/components/IPInfo.vue";
import leaflet from "leaflet";
import axios from "axios";
import { onMounted, ref } from "vue";

export default {
  name: "HomeView",
  components: { IPInfo },

  setup() {
    let map;

    const queryIp = ref("");
    const ipInfo = ref(null);

    onMounted(() => {
      map = leaflet.map("map").setView([51.505, -0.09], 13);

      leaflet
        .tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
          maxZoom: 19,
          attribution: "© OpenStreetMap",
        })
        .addTo(map);
    });

    const getIpInfo = async () => {
      try {
        const data = await axios.get(
          `https://geo.ipify.org/api/v2/country?apiKey=at_9suIeDSYDzY7gtXaQDUid8w7V6PAC&ipAddress=${queryIp.value}`
        );
        const result = data.data;
        console.log(data);
        ipInfo.value = {
          address: result.ip,
          state: result.location.region,
          timezone: result.location.timezone,
          isp: result.isp,
          lat: result.location.latlng,
          lng: result.location.latlng,
        };
        leaflet.marker([ipInfo.value.latlng, ipInfo.value.latlng]).addTo(map);
        map.setView([ipInfo.value.latlng, ipInfo.value.latlng], 13);
      } catch (e) {
        // alert(e.message);
        console.log(e);
      }
    };

    return {
      ipInfo,
      queryIp,
      getIpInfo,
    };
  },
};
</script>
