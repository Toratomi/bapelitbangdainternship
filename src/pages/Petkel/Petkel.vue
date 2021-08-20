<template>

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
    <div>
      <v-card>
    <v-toolbar
      dense
      floating
    >
      <v-text-field
        hide-details
        prepend-icon="mdi-magnify"
        single-line
      ></v-text-field>

      <v-btn icon>
        <v-icon>mdi-crosshairs-gps</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn>
    </v-toolbar>
  </v-card>
        <div id="map" style="width: 100%; height: 750px"></div>
    </div>
    

</template>

<script>
/*eslint-disable*/
import mapboxgl from "mapbox-gl";


let map;
export default {
  name: 'Petkel',
  mounted() {

    mapboxgl.accessToken =
      "pk.eyJ1IjoiZGVmcmFuayIsImEiOiJja3E4eTM0bHcwMnpqMnRvM2tjZjY2eWwzIn0.jWv6oKLTT9hY5WpkXlle6g";

    map = new mapboxgl.Map({
      container: "map", // container ID
      style: "mapbox://styles/mapbox/streets-v11", // style URL
      center: [124.8490833,1.4908421], // starting position [lng, lat]
      zoom: 16.8, // starting zoom
    });
    map.on('load', async function () {
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
          
          map.addSource("lingkungan", {
            type: "geojson",
            data: response,
          });
  
          map.addLayer({
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
  
          map.addLayer({
            id: "lingkungan" + '-borders',
            type: "line",
            source: "lingkungan",
            layout: {},
            paint: {
              "line-color": "#000000",
              "line-width": 1,
            },
          });
  
          let hoveredStateId = null;
          map.on("mousemove", "lingkungan" + '-fills', function (e) {
            if (e.features.length > 0) {
              if (hoveredStateId !== null) {
                map.setFeatureState(
                  { source: "lingkungan", id: hoveredStateId },
                  { hover: false }
                );
              }
              hoveredStateId = e.features[0].id;
              map.setFeatureState(
                { source: "lingkungan", id: hoveredStateId },
                { hover: true }
              );
            }
          });
  
          map.on("mouseleave", "lingkungan" + '-fills', function () {
            if (hoveredStateId !== null) {
              map.setFeatureState(
                { source: "lingkungan", id: hoveredStateId },
                { hover: false }
              );
            }
            hoveredStateId = null;
          });
  
          map.on('click', "lingkungan" + '-fills', function (e) {
            new mapboxgl.Popup()
            // .setLngLat(e.lngLat)
            .setHTML("no_bng" + e.features[0].properties.no_bng)
            .addTo(map);
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
