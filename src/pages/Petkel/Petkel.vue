
<template>
 <div>
    <!-- <div class="mapContainer">
      <GmapMap
        :center="{lat: 1.4907921, lng: 124.8484833}"
        :zoom="17"
        style="width: 100%; height: inherit"
      >
        <GmapMarker
          :position="{lat: 1.4907921, lng: 124.8484833}"
        />
      </GmapMap>
    </div> -->
      <!-- <div v-if="loading == 1">Loading Map...
      </div> -->
      <div  id="map" style="width: 100%; height: 600px">
      </div>
      <v-autocomplete @change="flyTo()" id="searchform" ref="searchform" v-model="bangunan_value" :items="bangunan" clearable></v-autocomplete>
      
      
      
      <div id="app" class="mt-2">
      <button class="button" @click="showModal = true, loadData()">
        Tampilkan Data Seluruh Pemilik
      </button>
      <transition name="fade" appear>
        <div class="modal-overlay" v-if="showModal" @click="showModal = false"></div>
      </transition>
      <transition name="slide" appear>
        <div class="modal" v-if="showModal">
          <v-simple-table>
                <template v-slot:default>
                  <thead>
                  <tr>
                    <th class="text-left pa-6">NAME</th>
                    <th class="text-left">EMAIL</th>
                    <th class="text-left">PRODUCT</th>
                    <th class="text-left">PRICE</th>
                    <th class="text-left">DATE</th>
                    <th class="text-left">CITY</th>
                    <th class="text-left">STATUS</th> >
                  </tr>
                  </thead>
                  
                </template>
              </v-simple-table>
          <button class="button" @click="showModal = false">
            close
          </button>
        </div>
      </transition>
    </div>
  </div>
</template>
<script>
/*eslint-disable*/
import mapboxgl from "mapbox-gl";
// import MapboxGeocoder from "@mapbox/mapbox-gl-geocoder"
import Header from '../../components/Header/Header.vue';
import * as turf from '@turf/turf';
import 'mapbox-gl/dist/mapbox-gl.css';
// import turf from '@turf/turf';

let inetApi = 'https://api.coingecko.com/api/v3/coins/list'
let geoportaldApi = "http://geoportal.manadokota.go.id/geoserver/wfs?srsName=EPSG%3A4326&typename=geonode%3Akelurahan_karame_kecsingkil_1&outputFormat=json&version=1.0.0&service=WFS&request=GetFeature&access_token=MlU8ffXvR9QQ2l2n2t4f3luLY0LQxh"

// let map;
window['map'];
export default {
  components: { Header, 
  },
  name: 'Petkel',
  data () {
    return {
      user: JSON.parse(localStorage.getItem('user')),
      showModal: false,
      bangunan: [],
      bangunan_value: '',
      loading: 0    
    }
  },
    
  methods: {
    flyTo() {
      let vueinstance = this;
      let featureitem = window['response_geojson'].features.find(feature => feature.properties.no_bng == vueinstance.bangunan_value);
      let centerfeature = turf.centroid(featureitem);
      window['map'].flyTo({
        center: centerfeature.geometry.coordinates,
        zoom: 20,
        essential: true // this animation is considered essential with respect to prefers-reduced-motion
      })
    },
    async loadData() {
      try {
        let response = await fetch(inetApi)
        console.log(response)
      } catch (error) {
        alert('Terjadi Kesalahan')
      }
    }
  },
  // try {
  //       window['load'] = await fetch('https://api.coingecko.com/api/v3/coins/list')
  //       console.log(response)
  //     } catch (error) {
  //       alert('Terjadi Kesalahan')
  //     }
  // window['map'].flyTo({
  //             center: [
  //               124.8490833 + (Math.random() - 0.5) * 10,
  //               1.4908421 + (Math.random() - 0.5) * 10
  //             ],
  //             
  //           });
  mounted() {
    mapboxgl.accessToken =
      "pk.eyJ1IjoiZGVmcmFuayIsImEiOiJja3E4eTM0bHcwMnpqMnRvM2tjZjY2eWwzIn0.jWv6oKLTT9hY5WpkXlle6g";

    window['map'] = new mapboxgl.Map({
      container: "map", // container ID
      style: "mapbox://styles/mapbox/streets-v11", // style URL
      center: [124.8490833,1.4908421], // starting position [lng, lat]
      zoom: 16.8, // starting zoom
    });

    console.log(this.$refs);
    let vueins = this;


    // fetch("http://192.168.1.6:8000/api/login")
    // .then(function(response) {
    //   console.log(response)
    // })

    let vuecomponent = this;
    
    window['map'].on('load', async function () {
      console.log(vueins.$refs)
      try {
        let response = await fetch (geoportaldApi);
        response = await response.json();

        response.features = response.features.map(featureItem => {
          let newfeatureItem = {...featureItem};
          newfeatureItem.id = "kelurahan_karame_kecsingkil_1" + newfeatureItem.id
          return newfeatureItem
        })

        window['response_geojson'] = response;

        console.log(window['response_geojson']);

        let bangunan = response.features.map(featureItems => featureItems.properties.no_bng);

        vuecomponent.bangunan = bangunan;

        let lingkungans = response.features.map(featureItem => featureItem.properties.lingkungan);
          
        lingkungans = new Set(lingkungans);
        // console.log(lingkungans);
        lingkungans = Array.from(lingkungans);
        // console.log(lingkungans);
  
        lingkungans = lingkungans.map(lingkunganitem => {
          let fillColor = "#";
          let hexaNum = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F'];
          for (let colorIndex = 1; colorIndex <= 6; colorIndex++) {
            let randomNumber = Math.floor(Math.random() * hexaNum.length);
            fillColor += hexaNum[randomNumber];
          }
  
          let myresponse = {...response};
          myresponse.features = myresponse.features.filter(myfeatureItem => myfeatureItem.properties.lingkungan == lingkunganitem);
            
          return {
            nama: lingkunganitem,
            fillColor,
            // response: myresponse
          };
        });
        console.log(lingkungans);
  
        response.features = response.features.map((featureItem, featureIndex) => {
          let checkColorLingk = lingkungans.find(lingkItem => lingkItem.nama == featureItem.properties.lingkungan);
          let resultColor = "#ffffff";
          if (checkColorLingk) {
            resultColor = checkColorLingk.fillColor; 
          }
          return {
            id: featureIndex + 1,
            type: featureItem.type,
            geometry: featureItem.geometry,
            properties: {
              ...featureItem.properties,
              lingkunganColor: resultColor
            }
          }
        });
        window['map'].addSource("lingkungan", {
          type: "geojson",
          data: response,
        });
  
        window['map'].addLayer({
          id: "lingkungan" + '-fills',
          type: "fill",
          source: "lingkungan",
          layout: {},
          paint: {
            "fill-color": ['string', ['get', "lingkunganColor"], "#fffff"],
            "fill-opacity": [
              "case",
              ["boolean", ["feature-state", "hover"], false],
              1,
              0.5,
            ],
          },
        });
  
        window['map'].addLayer({
          id: "lingkungan" + '-borders',
          type: "line",
          source: "lingkungan",
          layout: {},
          paint: {
            "line-color": "#000000",
            "line-width": [
              "case",
              ["boolean", ["feature-state", "click"], false],
              10,
              0.5,
            ],
          },
        });

        map.addLayer({
          id: 'lingkungan',
          type: 'symbol',
          source: 'lingkungan',
          layout: {}
        });
  
        let hoveredStateId = null;
        window['map'].on("mousemove", "lingkungan" + '-fills', function (e) {
          if (e.features.length > 0) {
            if (hoveredStateId !== null) {
              window['map'].setFeatureState(
                { source: "lingkungan", id: hoveredStateId },
                { hover: false }
              );
            }
            hoveredStateId = e.features[0].id;
            window['map'].setFeatureState(
              { source: "lingkungan", id: hoveredStateId },
              { hover: true }
            );
          }
        });
  
        window['map'].on("mouseleave", "lingkungan" + '-fills', function () {
          if (hoveredStateId !== null) {
            window['map'].setFeatureState(
              { source: "lingkungan", id: hoveredStateId },
              { hover: false }
            );
          }
          hoveredStateId = null;
        });
          
          // console.log(window['map'])
        let clickedStateId = null;
        window['map'].on("click", "lingkungan" + '-fills', function (e) {
          clickedStateId = e.features[0].id;
          console.log(clickedStateId)
          console.log(window['response_geojson'])
          if (clickedStateId !== null) {
            let data_bangunan = fetch('http://192.168.0.114:8000/api/bangunan', {
              method: 'POST',
              body: JSON.stringify({
                remember_token: vuecomponent.user.remember_token,
                bangunan_id: clickedStateId
              }),
              headers:{
                'content-type':'application/json'
              },
            });
            console.log(data_bangunan)
            let featureitem = e.features.find(feature => feature.id == clickedStateId);
            let centerfeature = turf.centroid(featureitem);
            let lnglat = centerfeature.geometry.coordinates;
            let popup = new mapboxgl.Popup();
            popup.setLngLat(lnglat)
            .setHTML(`<table> 
                    <tr>
                        <td>ID</td>
                        <td>:</td>
                        <td>${clickedStateId}</td>
                    </tr>
                    <tr>
                        <td>Name</td>
                        <td>:</td>
                        <td>${clickedStateId}</td>
                    </tr>
                    </table>
                    <button type="button" onclick="alert('Success on ${clickedStateId} ${clickedStateId})">Detail</button>`)
            .addTo(window['map']);
            window['map'].flyTo({
              center: centerfeature.geometry.coordinates,
              zoom: 20,
              essential: true // this animation is considered essential with respect to prefers-reduced-motion
            });
          }
        });
      } 
      catch (error) {
          console.log(error);
      }
    })
  },
  
};

</script>

<style src="./Petkel.scss" lang="scss" scoped/>
