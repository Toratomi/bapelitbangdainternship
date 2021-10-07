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
    <v-card>
      <div  id="map" style="width: 100%; height: 600px"></div>
    </v-card>
    <v-card>
      <div  v-if="loadLayer" class="mt-4">
        <v-autocomplete  @change="flyTo()" id="searchform" ref="searchform" v-model="nik_value" :items="nik" clearable></v-autocomplete>
      </div>
      <div v-else>
        <v-progress-linear
          indeterminate
          color="sik"
        ></v-progress-linear>
      </div>
    </v-card>
    <v-card class="mt-4">
      <v-col
          cols="12">
        <v-data-table
          :headers="headers"
          :items="Bangunan_pilihan.penghuni"
          :search="search"
          :loading="isLoading"
          loading-text="Loading... Please wait"
          class="elevation-1"
          ><template v-slot:top>
            <v-toolbar flat>
              <v-toolbar-title class="text-h6 black--text font-weight-black">Daftar Data Penghuni</v-toolbar-title>
              <v-divider
                class="mx-4"
                inset
                vertical
              ></v-divider> 
              <v-text-field
                v-model="search"
                append-icon="mdi-magnify"
                label="Cari"
                single-line
                hide-details
              ></v-text-field>
            </v-toolbar>
          </template>
          <template v-slot:no-data>
            <div>Silahkan memilih Bangunan</div>
          </template>
        </v-data-table>
      </v-col>
    </v-card>
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
      nik: [],
      loadLayer: 0,
      isLoading: false,
      loadPenduduk: 0,
      hide: true,
      nik_value: '',
      loading: 0,
      search: '',
        headers: [
          {
            text: 'Nama',
            align: 'start',
            sortable: true,
            value: 'nama',
          },
          { text: 'Rumah', value: 'bangunan_id' },
          { text: 'Tanggal Lahir', value: 'tanggal_lahir' },
          { text: 'Tempat Lahir', value: 'tempat_lahir' },
          { text: 'No. KK', value: 'nomor_kk' },
          { text: 'NIK', value: 'nik' },
          { text: 'No. telp', value: 'nomor_telepon' },
          { text: 'Email', value: 'email' },
          { text: 'Jenis Kelamin', value: 'jenis_kelamin' },
          { text: 'Status Pernikahan', value: 'status_pernikahan' },
        ],
        Bangunan_pilihan: [],    
    }
  },
    
  methods: {
    async flyTo() {
      try {
        let vueinstance = this;
        vueinstance.loadLayer = true;
        let nikID = window['penduduks'].penduduk.find(nikID => nikID.nik == vueinstance.nik_value)
        console.log(nikID.bangunan_id)
        let nomor_bangunan = await fetch('http://192.168.43.197:8000/api/bangunan/search', {
            method: 'POST',
            body: JSON.stringify({
              remember_token: vueinstance.user.remember_token,
              bangunan_id: nikID.bangunan_id
            }),
            headers:{
              'content-type':'application/json'
            },
          });
        nomor_bangunan = await nomor_bangunan.json()
        console.log(nomor_bangunan.bangunan_id)
        vueinstance.loadPenduduk == 1
        let featureitem = window['response_geojson'].features.find(feature => feature.properties.no_bng == nomor_bangunan.bangunan_id);
        let centerfeature = turf.centroid(featureitem);
        window['map'].flyTo({
          center: centerfeature.geometry.coordinates,
          zoom: 21,
          essential: true // this animation is considered essential with respect to prefers-reduced-motion
        })
        vueinstance.loadPenduduk == 2
      } catch (error) {
        console.log(error)
      }
      
    },
    // async loadData() {
    //   try {
    //     let response = await fetch(inetApi)
    //     console.log(response)
    //   } catch (error) {
    //     alert('Terjadi Kesalahan')
    //   }
    // }
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
        window['penduduks'] = await fetch('http://192.168.43.197:8000/api/penduduk/show', {
          method: 'POST',
          body: JSON.stringify({
            remember_token: vuecomponent.user.remember_token,
          }),
          headers:{
            'content-type':'application/json'
          },
        });
        window['penduduks'] = await window['penduduks'].json();
        let nik = window['penduduks'].penduduk.map(featureitem => featureitem.nik);
        vuecomponent.nik = nik
          // console.log(response)
          this.perpenduduk = response
        let response = await fetch (geoportaldApi);
        response = await response.json();
        response.features = response.features.map(featureItem => {
          let newfeatureItem = {...featureItem};
          newfeatureItem.id = "kelurahan_karame_kecsingkil_1" + newfeatureItem.id
          return newfeatureItem
        })

        window['data_bangunan'] = await fetch('http://192.168.43.197:8000/api/bangunan/map', {
              method: 'POST',
              body: JSON.stringify({
                remember_token: vuecomponent.user.remember_token,
                bangunan_id: clickedStateId
              }),
              headers:{
                'content-type':'application/json'
              },
            });
            window['data_bangunan'] = await window['data_bangunan'].json()
            console.log(window['data_bangunan'].daftar_bangunan.map(featureItem => featureItem.bangunan.id))

            

        window['response_geojson'] = response;
        

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
        vuecomponent.loadLayer = 0;
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
        vuecomponent.loadLayer = 1;
  
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
          data_bangunan = window['data_bangunan']
          console.log(data_bangunan)
          // console.log(window['map'])
        let clickedStateId = null;
        window['map'].on("click", "lingkungan" + '-fills', function (e)  {
          clickedStateId = e.features[0].id;
          console.log(clickedStateId)
          if (clickedStateId !== null) {
            // vuecomponent.loadLayer = true;
            // console.log(vuecomponent.loadLayer)
            
            let bangunanitem = window['data_bangunan'].daftar_bangunan.find(featureItem => featureItem.bangunan.id == clickedStateId)
            vuecomponent.Bangunan_pilihan = bangunanitem
            let featureitem = e.features.find(feature => feature.id == clickedStateId);
            let centerfeature = turf.centroid(featureitem);
            let lnglat = centerfeature.geometry.coordinates;
            let popup = new mapboxgl.Popup();
            popup.setLngLat(lnglat)
            .setHTML(`<table> 
                    <tr>
                        <td>Pemilik</td>
                        <td>:</td>
                        <td>${bangunanitem.bangunan.nama_pemilik}</td>
                    </tr>
                    <tr>
                        <td>Nik Pemilik</td>
                        <td>:</td>
                        <td>${bangunanitem.bangunan.nik_pemilik}</td>
                    </tr>
                    <tr>
                        <td>Jumlah Penghuni</td>
                        <td>:</td>
                        <td>${bangunanitem.jumlah_penghuni}</td>
                    </tr>
                    </table>
                    <button type="button" onclick="('Success on ${console.log(clickedStateId)} ${clickedStateId})">Detail</button>`)
            .addTo(window['map']);
            window['map'].flyTo({
              center: centerfeature.geometry.coordinates,
              zoom: 21,
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
