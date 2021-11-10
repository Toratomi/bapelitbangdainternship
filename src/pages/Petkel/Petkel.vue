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
        <v-autocomplete  @change="flyTo()" id="searchform" ref="searchform" v-model="nik_value" :items="nik" label="Cari NIK Penduduk" clearable></v-autocomplete>
      </div>
      <div v-else>
        <v-progress-linear
          indeterminate
          color="sik"
        ></v-progress-linear>
      </div>
    </v-card>
    <v-card class="mt-4" v-if="clickedStateId">
      <v-col
          cols="12">
        <v-data-table
          :headers="headers"
          :items="Bangunan_pilihan.penghuni"
          :search="search"
          :loading="isLoading"
          loading-text="Memuat... Mohon Bersabar"
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
              <v-dialog
                v-model="dialogAdd"
                max-width="500px"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    color="sik"
                    class="mb-2 white--text"
                    rounded
                    v-bind="attrs"
                    v-on="on"
                  >
                    <v-icon>mdi-plus</v-icon>
                    Tambah Penghuni
                  </v-btn>
                </template> 
                <v-card>
                  <v-card-title>
                    <span class="text-h5">Tambah Penghuni</span>
                  </v-card-title>
                  <v-card-text>
                    <v-container>
                      <v-row>
                        <v-col
                          cols="12"
                          sm="6"
                          md="5"
                        >
                          <v-text-field
                            v-model="editedItem.nama"
                            label="Nama"
                            placeholder="Masukkan Nama"
                            onkeypress="return (event.charCode > 64 && event.charCode < 91) || (event.charCode > 96 && event.charCode < 123) || (event.charCode==32)"
                            required
                          ></v-text-field>
                        </v-col>
                        <v-spacer></v-spacer>
                        <v-col
                          cols="12"
                          sm="6"
                          md="5"
                        >
                          <v-autocomplete
                            v-model="clickedStateId"
                            :items="clickedStateId"
                            item-text="id"
                            item-value="id"
                            label="ID Rumah"
                            placeholder="Masukkan ID Rumah"
                            required
                          >
                          </v-autocomplete>
                        </v-col>
                        <v-col
                          cols="12"
                          sm="6"
                          md="5"
                        >
                          <v-text-field
                            type="date"
                            placeholder="Masukkan Tanggal Lahir"
                            v-model="editedItem.tanggal_lahir"
                            label="Tanggal Lahir"
                            required
                          ></v-text-field>
                        </v-col>
                        <v-spacer></v-spacer>
                        <v-col
                          cols="12"
                          sm="6"
                          md="5"
                        >
                          <v-text-field
                            v-model="editedItem.tempat_lahir"
                            label="Tempat Lahir"
                            placeholder="Masukkan Tempat Lahir"
                            onkeypress="return (event.charCode > 64 && event.charCode < 91) || (event.charCode > 96 && event.charCode < 123) || (event.charCode==32)"
                            required
                          ></v-text-field>
                        </v-col>
                        <v-col
                          cols="12"
                          sm="6"
                          md="5"
                        >
                          <v-text-field
                            v-model="editedItem.nomor_kk"
                            label="No. KK"
                            placeholder="Masukkan Nomor KK"
                            type="number"
                            required
                          ></v-text-field>
                        </v-col>
                        <v-spacer></v-spacer>
                        <v-col
                          cols="12"
                          sm="6"
                          md="5"
                        >
                          <v-text-field
                            v-model="editedItem.nik"
                            label="NIK"
                            placeholder="Masukkan NIK"
                            type="number"
                            required
                          ></v-text-field>
                        </v-col>
                        <v-col
                          cols="12"
                          sm="6"
                          md="5"
                        >
                          <v-text-field
                            v-model="editedItem.nomor_telepon"
                            label="No. Telp"
                            placeholder="Masukkan Nomor Telepon"
                            type="number"
                            required
                          ></v-text-field>
                        </v-col>
                        <v-spacer></v-spacer>
                        <v-col
                          cols="12"
                          sm="6"
                          md="5"
                        >
                          <v-text-field
                            v-model="editedItem.email"
                            label="Email"
                            placeholder="Masukkan Email"
                            type="email"
                            :rules="emailRules"
                            required
                          ></v-text-field>
                        </v-col>
                        <v-col
                          cols="12"
                          sm="6"
                          md="5"
                        >
                          <v-autocomplete
                            v-model="editedItem.jenis_kelamin"
                            :items="gender"
                            :search-input="search"
                            item-text="nama"
                            item-value="nama"
                            label="Jenis Kelamin"
                            placeholder="Jenis Kelamin"
                            required
                          >
                          </v-autocomplete>
                        </v-col>
                        <v-spacer></v-spacer>
                        <v-col
                          cols="12"
                          sm="6"
                          md="5"
                        >
                          <v-autocomplete
                            v-model="editedItem.status_pernikahan"
                            :items="status"
                            item-text="nama"
                            item-value="nama"
                            label="Status Pernikahan"
                            placeholder="Status Pernikahan"
                            required 
                          >
                          </v-autocomplete>
                        </v-col>
                      </v-row>
                    </v-container>
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn
                      color="sik darken-1"
                      text
                      @click="closeAdd"
                    >
                      Batal
                    </v-btn>
                    <v-btn
                      color="sik darken-1"
                      text
                      @click="verifikasiAdd=true"
                    >
                      Tambah
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
              <v-dialog
                v-model="verifikasiAdd"
                max-width="800px"
              >
                <v-card>
                  <v-card-title>
                    <span class="text-h5">Apakah benar akan menambahkan <strong>{{editedItem.nama}} </strong> pada data penduduk?</span>
                  </v-card-title>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn
                      color="sik darken-1"
                      text
                      @click="verifikasiAdd=false"
                    >
                      Batal
                    </v-btn>
                    <v-btn
                      color="sik darken-1"
                      text
                      @click="verifikasiAdd=false, closeAdd(), add()"
                    >
                      Konfirmasi
                    </v-btn>
                    <v-spacer></v-spacer>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </v-toolbar>
          </template>
          <template v-slot:no-data>
            <div>Bangunan Tidak memiliki Penghuni</div>
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
      emailRules: [
          v => !!v || 'E-mail is required',
          v => /.+@.+/.test(v) || 'E-mail must be valid',
        ],
      bangunan: [],
      nik: [],
      loadLayer: 0,
      isLoading: false,
      nik_value: '',
      loading: 0,
      search: '',
      dialogAdd: false,
      verifikasiAdd: false,
      clickedStateId: null,
      Bangunan_pilihan: [],
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
      gender: [ 'Laki-laki', 'Perempuan',],
      status: [ 'Belum Menikah', 'Sudah Menikah','Janda/Duda'],
      editedIndex: -1,
      editedItem: {
        nama: '',
        bangunan_id: '',
        tanggal_lahir: '',
        tempat_lahir: '',
        nomor_kk: '',
        nik: '',
        nomor_telepon: '',
        email: '',
        jenis_kelamin: '',
        status_pernikahan: '',
      },   
    }
  },
  

  watch: {
      dialogAdd (val) {
        val || this.close()
      },
      verifikasiAdd (val) {
        val || this.close()
      },
    },
    
  methods: {
    async flyTo() {
      try {
        let vueinstance = this;
        vueinstance.loadLayer = true;
        let nikID = window['penduduks'].penduduk.find(nikID => nikID.nik == vueinstance.nik_value)
        console.log(nikID.bangunan_id)
        
        let nomor_bangunan = await fetch('http://192.168.1.20:8000/api/bangunan/search', {
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
        
        let featureitem = window['response_geojson'].features.find(feature => feature.properties.no_bng == nomor_bangunan.bangunan_id);
        let centerfeature = turf.centroid(featureitem);
        window['map'].flyTo({
          center: centerfeature.geometry.coordinates,
          zoom: 21,
          essential: true // this animation is considered essential with respect to prefers-reduced-motion
        })
      } catch (error) {
        console.log(error)
      }
      
    },
    close () {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },
    closeAdd () {
      this.dialogAdd = false
    },
    add() {
      console.log(this.editedItem)
      try {
        fetch('http://192.168.1.20:8000/api/penduduk/map/create', {
        method: 'POST',
        body: JSON.stringify({
          remember_token: this.user.remember_token,
          nama: this.editedItem.nama,
          bangunan_id: this.clickedStateId,
          tanggal_lahir: this.editedItem.tanggal_lahir,
          tempat_lahir: this.editedItem.tempat_lahir,
          nomor_kk: this.editedItem.nomor_kk,
          nik: this.editedItem.nik,
          nomor_telepon: this.editedItem.nomor_telepon,
          email: this.editedItem.email,
          jenis_kelamin: this.editedItem.jenis_kelamin,
          status_pernikahan: this.editedItem.status_pernikahan,
        }),
        headers:{
          'content-type':'application/json'
        },
      });
      } catch (error) {
        alert('Terjadi Kesalahan')
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
        window['penduduks'] = await fetch('http://192.168.1.20:8000/api/penduduk/show', {
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
        console.log(window['penduduks'])
          // console.log(response)
        this.perpenduduk = response
        let response = await fetch (geoportaldApi);
        response = await response.json();
        response.features = response.features.map(featureItem => {
          let newfeatureItem = {...featureItem};
          newfeatureItem.id = "kelurahan_karame_kecsingkil_1" + newfeatureItem.id
          return newfeatureItem
        })

        window['data_bangunan'] = await fetch('http://192.168.1.20:8000/api/bangunan/map', {
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
          vuecomponent.clickedStateId = clickedStateId
          console.log(clickedStateId)
          if (clickedStateId !== null) {
            vuecomponent.isLoading = true
            // vuecomponent.loadLayer = true;
            // console.log(vuecomponent.loadLayer)
            
            let bangunanitem = window['data_bangunan'].daftar_bangunan.find(featureItem => featureItem.bangunan.id == clickedStateId)
            vuecomponent.Bangunan_pilihan = bangunanitem
            console.log(bangunanitem.bangunan.id)
            let featureitem = e.features.find(feature => feature.id == clickedStateId);
            let centerfeature = turf.centroid(featureitem);
            let lnglat = centerfeature.geometry.coordinates;
            let popup = new mapboxgl.Popup();
            popup.setLngLat(lnglat)
            .setHTML(`<table> 
                    <tr>
                        <td><b>Pemilik</b></td>
                        <td>:</td>
                        <td>${bangunanitem.bangunan.nama_pemilik}</td>
                    </tr>
                    <tr>
                        <td><b>Nik Pemilik</b></td>
                        <td>:</td>
                        <td>${bangunanitem.bangunan.nik_pemilik}</td>
                    </tr>
                    <tr>
                        <td><b>Jumlah Penghuni</b></td>
                        <td>:</td>
                        <td>${bangunanitem.jumlah_penghuni}</td>
                    </tr>
                    <tr>
                        <td><b>Bangunan ID</b></td>
                        <td>:</td>
                        <td>${clickedStateId}</td>
                    </tr>
                    </table>`)
            .addTo(window['map']);
            window['map'].flyTo({
              center: centerfeature.geometry.coordinates,
              zoom: 21,
              essential: true // this animation is considered essential with respect to prefers-reduced-motion
            });
            vuecomponent.isLoading = false
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
