
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
      <button id="fly">Fly</button>
      <div id="map" style="width: 100%; height: 750px">
      </div>
      <v-autocomplete @change="flyTo()" id="searchform" ref="searchform" v-model="lingkungans" :items="lingkungans" :search="search" item-text="nama" item-value="fillcolor"></v-autocomplete>
      
      <!-- <div id="app" class="mt-2">
      <button class="button" @click="showModal = true">
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
    </div> -->
  </div>
</template>
<script>
/*eslint-disable*/
import mapboxgl from "mapbox-gl";
// import MapboxGeocoder from "@mapbox/mapbox-gl-geocoder"
import Header from '../../components/Header/Header.vue';
import turf from '@turf/centroid';


// let map;
window['map']
export default {
  components: { Header, 
  },
  name: 'Petkel',
  data () {
    return {
      showModal: false,
      lingkungans: ['tes', '2'],
      search: ''
    }
  },
    
  methods: {
    flyTo() {
      window['map'].flyTo({
              center: [
                124.8490833 + (Math.random() - 0.5) * 10,
                1.4908421 + (Math.random() - 0.5) * 10
              ],
              essential: true // this animation is considered essential with respect to prefers-reduced-motion
            });
    }
  },
  mounted() {

    mapboxgl.accessToken =
      "pk.eyJ1IjoiZGVmcmFuayIsImEiOiJja3E4eTM0bHcwMnpqMnRvM2tjZjY2eWwzIn0.jWv6oKLTT9hY5WpkXlle6g";

    window['map'] = new mapboxgl.Map({
      container: "map", // container ID
      style: "mapbox://styles/mapbox/streets-v11", // style URL
      center: [124.8490833,1.4908421], // starting position [lng, lat]
      zoom: 16.8, // starting zoom
    });

    console.log(this.$refs)
    let vueins = this;


    fetch("petkel/karame_center_coordinates.json")
    .then(function(response) {
      console.log(response)
    })

    window['map'].on('load', async function () {
      console.log(vueins.$refs)
      try {
          let response = await fetch ("http://geoportal.manadokota.go.id/geoserver/wfs?srsName=EPSG%3A4326&typename=geonode%3Akelurahan_karame_kecsingkil_1&outputFormat=json&version=1.0.0&service=WFS&request=GetFeature&access_token=MlU8ffXvR9QQ2l2n2t4f3luLY0LQxh");
          response = await response.json();
  
          console.log(response);
  
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
          // let geocoder = new MapboxGeocoder({
          //   // Initialize the geocoder
          //   accessToken: mapboxgl.accessToken, // Set the access token
          //   mapboxgl: mapboxgl, // Set the mapbox-gl instance
          //   marker: false // Do not use the default marker style
          // });

            // Add the geocoder to the map
          // window['map'].addControl(geocoder);
          document.getElementById('searchform').addEventListener('change', () => {
            // Fly to a random location by offsetting the point -74.50, 40
            // by up to 5 degrees.
            window['map'].flyTo({
              center: [
                124.8490833 + (Math.random() - 0.5) * 10,
                1.4908421 + (Math.random() - 0.5) * 10
              ],
              essential: true // this animation is considered essential with respect to prefers-reduced-motion
            });
          });

          document.getElementById('fly').addEventListener('click', () => {
            // Fly to a random location by offsetting the point -74.50, 40
            // by up to 5 degrees.
            window['map'].flyTo({
              center: [
                124.8490833 + (Math.random() - 0.5) * 10,
                1.4908421 + (Math.random() - 0.5) * 10
              ],
              essential: true // this animation is considered essential with respect to prefers-reduced-motion
            });
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
              "line-width": 1,
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
          // window['map'].on("click", "lingkungan" + '-fills', function () {
          //   // new mapboxgl.Popup(<h1>test</h1>)
          //   let template = '<h1>Test<h1>'
          //   window['map'].bindPopup(template);
          //   // .setLngLat(e.lngLat)
          // });
          // let test = '<h1>Test<h1>'
          // window['map'].bindPopup(test)
  
          // window['map'].on('click', "lingkungan" + '-fills', function (e) {
          //   let coordinates = e.features[0].geometry.coordinates.slice();
          //   let bangunan = e.features[0].properties.no_bng


          //   new mapboxgl.Popup()
          //   .setLngLat(coordinates)
          //   .setHTML(bangunan)
          //   // .setHTML("no_bng" + e.features[0].properties.no_bng)
          //   .addTo(window['map']);
          // });
           window['map'].on('click', "lingkungan" + '-fills', function (e) {
             let coordinates = e.features[0].geometry.coordinates
             let test = lnglat(coordinates)
             let center = test.getCenter()
            //  let centroid = turf.centroid(coordinates)
             console.log(coordinates)
             console.log(test)
             console.log(center)
            //  consol.log(centroid)
             window['map'].flyTo({
               
               center: e.features[0].geometry.coordinates
             });
          });


          let featCollection = {
            type: 'FeatureCollection',
            features: response.features.geometry
          }
         

          // Magic happens here:
          let centroid = turf.centroid(featCollection);


          // L.geoJSON(featCollection).addTo(window['map'])

          // L.geoJSON(centroid).addTo(window['map']);
          
          window['map'].addSource("featCollection", {
            type: "geojson",
            data: centroid,
          });

          // // window['map'].addSource("featCollection", {
          // //   type: "geojson",
          // //   data: response,
          // // });
          // // window['map'].addSource("centroid", {
          // //   type: "geojson",
          // //   data: response,
          // // });
  
          // window['map'].addLayer({
          //   id: "featCollection" + '-fills',
          //   type: "circle",
          //   source: "featCollection",
          //   layout: {},
          //   paint: {
          //     "fill-color": ['string', ['get', "lingkunganColor"], "#fffff"],
          //     "fill-opacity": [
          //       "case",
          //       ["boolean", ["feature-state", "hover"], false],
          //       1,
          //       0.5,
          //     ],
          //   },
          // });

          // window['map'].addLayer({
          //   id: "featCollection" + '-borders',
          //   type: "line",
          //   source: "featCollection",
          //   layout: {},
          //   paint: {
          //     "line-color": "#000000",
          //     "line-width": 1,
          //   },
          // });
  

      } 
      catch (error) {
          console.log(error);
      }
    })
  },
  
};

</script>

<style src="./Petkel.scss" lang="scss" scoped/>
