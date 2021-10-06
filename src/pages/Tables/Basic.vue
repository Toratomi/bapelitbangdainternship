<template>
<v-container fluid>
    <div class="tables-basic">
        <v-row>
            <!-- <v-col cols="12">
          <v-card class="employee-list mb-1">
            <v-card-title class="pa-6 pb-3">
              <p>Employee List</p>
              <v-spacer></v-spacer>
              <v-text-field
                v-model="mock.employeeTable.search"
                append-icon="mdi-magnify"
                label="Search"
                clearable
                single-line
                hide-details
              ></v-text-field>
            </v-card-title>
            <v-data-table
              v-model="mock.employeeTable.selected"
              :headers="mock.employeeTable.headers"
              :items="mock.employeeTable.employee"
              :search="mock.employeeTable.search"
              item-key="name"
              show-select>
            </v-data-table>
          </v-card>
        </v-col> -->
            <v-col cols=12>
                <v-card class="mb-1">
                    <v-card-title class="white">
                        <span class="text-h5 black--text font-weight-black">Identitas Kelurahan</span>
                        <v-spacer></v-spacer>
                        <v-menu>
                            <template v-slot:activator="{ on, attrs }">
                                <v-btn icon v-bind="attrs" v-on="on"  @click="showModal = true">
                                    <v-icon color="grey">mdi-pencil</v-icon>
                                </v-btn>
                            </template>
                            
                        </v-menu>
                    </v-card-title>
                    <v-simple-table>
                        <template v-slot:default>
                            <thead>
                                <tr>
                                    <!--<th class="text-left pa-6">NAME</th>
                    <th class="text-left">EMAIL</th>
                     <th class="text-left">PRODUCT</th>
                    <th class="text-left">PRICE</th>
                    <th class="text-left">DATE</th>
                    <th class="text-left">CITY</th>
                    <th class="text-left">STATUS</th> -->
                                </tr>
                            </thead>
                            <tbody>
                                <tr>
                                    <th scope="row" class="text-body-1">Nama Kelurahan</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="kelurahan">{{ kelurahans.kelurahan.kelurahan }}</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Kode Kelurahan</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="id">{{kelurahans.kelurahan.id }}</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Kecamatan</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="kecamatan">{{ kelurahans.kelurahan.kecamatan }}</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Kabupaten/Kota</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="kabupaten_kota">{{ kelurahans.kelurahan.kabupaten_kota }}</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Provinsi</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="provinsi">{{ kelurahans.kelurahan.provinsi }}</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Alamat Kantor Kelurahan</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="alamat_kantor">{{ kelurahans.kelurahan.alamat_kantor }}</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Telepon Kelurahan</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="telepon_kelurahan">{{ kelurahans.kelurahan.telepon_kelurahan }}</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Email Kelurahan</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="email_kelurahan">{{ kelurahans.kelurahan.email_kelurahan }}</span></td>
                                    <!--tr v-for="item in mock.materialUITable" :key="item.name">
                    <td class="pa-6">{{ item.name }}</td>
                    <td>{{ item.email }}</td>
                    < <td>{{ item.product }}</td>
                    <td>{{ item.price }}</td>
                    <td>{{ item.date }}</td>
                    <td>{{ item.city }}</td>
                    <td v-if="item.status === 'Sent'">
                      <v-chip
                        link
                        color="success"
                        class="ma-2 ml-0"
                      >
                        Sent
                      </v-chip>
                    </td>
                    <td v-else-if="item.status === 'Pending'">
                      <v-chip
                        link
                        color="warning"
                        class="ma-2 ml-0"
                      >
                        Pending
                      </v-chip>
                    </td>
                    <td v-else-if="item.status === 'Declined'">
                      <v-chip
                        link
                        color="secondary"
                        class="ma-2 ml-0"
                      >
                        Declined
                      </v-chip>
                    </td> -->
                                </tr>
                            </tbody>
                            <!-- <tbody v-else>
                                <tr>
                                    <th scope="row" class="text-body-1">Nama Kelurahan</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="kelurahan">loading</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Kode Kelurahan</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="id">loading</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Kecamatan</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="kecamatan">loading</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Kabupaten/Kota</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="kabupaten_kota">loading</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Provinsi</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="provinsi">loading</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Alamat Kantor Kelurahan</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="alamat_kantor">loading</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Telepon Kelurahan</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="telepon_kelurahan">loading</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Email Kelurahan</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="email_kelurahan">loading</span></td> -->
                                    <!--tr v-for="item in mock.materialUITable" :key="item.name">
                    <td class="pa-6">{{ item.name }}</td>
                    <td>{{ item.email }}</td>
                    < <td>{{ item.product }}</td>
                    <td>{{ item.price }}</td>
                    <td>{{ item.date }}</td>
                    <td>{{ item.city }}</td>
                    <td v-if="item.status === 'Sent'">
                      <v-chip
                        link
                        color="success"
                        class="ma-2 ml-0"
                      >
                        Sent
                      </v-chip>
                    </td>
                    <td v-else-if="item.status === 'Pending'">
                      <v-chip
                        link
                        color="warning"
                        class="ma-2 ml-0"
                      >
                        Pending
                      </v-chip>
                    </td>
                    <td v-else-if="item.status === 'Declined'">
                      <v-chip
                        link
                        color="secondary"
                        class="ma-2 ml-0"
                      >
                        Declined
                      </v-chip>
                    </td> -->
                                <!-- </tr>
                            </tbody> -->
                        </template>
                    </v-simple-table>
                </v-card>

            </v-col>
        </v-row>

    </div>
    <div class="row">
        <div class="col-sm-6 v-center">
            <v-card>
                <v-img src="https://media.suara.com/pictures/970x544/2019/01/05/11583-calon-presiden-fiktif-nurhadi-instagram-atnurhadi-aldo.jpg" height="200px">
                </v-img>
                <v-card-title class="white">
                    <span class="text-h8 black--text">Lurah</span>
                    <v-spacer></v-spacer>
                    <v-menu>
                            <template v-slot:activator="{ on, attrs }">
                                <v-btn icon v-bind="attrs" v-on="on"  @click="showlurahModal = true">
                                    <v-icon color="grey">mdi-pencil</v-icon>
                                </v-btn>
                            </template>
                            
                        </v-menu>
                   
                </v-card-title>
                <v-list>
                  <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-account</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">NIP : {{kelurahans.kelurahan.lurah.nip}}</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-account</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">{{kelurahans.kelurahan.lurah.nama}}</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                    <v-divider inset></v-divider>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-phone</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">{{kelurahans.kelurahan.lurah.nomor_telepon}}</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                    <v-divider inset></v-divider>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-email</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">{{kelurahans.kelurahan.lurah.email}}</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                </v-list>
                <!-- <v-list v-else>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-account</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">loading</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                    <v-divider inset></v-divider>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-phone</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">loading</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                    <v-divider inset></v-divider>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-email</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">loading</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                </v-list> -->
            </v-card>
        </div>
        <div class="col-sm-6">
            <v-card>
                <v-img src="https://dcywrb6nqrsdp.cloudfront.net/attachment/77460591621636229908.large" height="200px">
                </v-img>
                <v-card-title class="white-3">
                    <span class="text-h8 black--text font-weight-bold">Sekretaris Lurah</span>
                    <v-spacer></v-spacer>
                    <v-menu>
                            <template v-slot:activator="{ on, attrs }">
                                <v-btn icon v-bind="attrs" v-on="on"  @click="showsekretarisModal = true">
                                    <v-icon color="grey">mdi-pencil</v-icon>
                                </v-btn>
                            </template>
                            
                        </v-menu>
                </v-card-title>
                <v-list >
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-account</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">NIP : {{kelurahans.kelurahan.sekretaris.nip}}</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                     <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-account</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">{{kelurahans.kelurahan.sekretaris.nama}}</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                    <v-divider inset></v-divider>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-phone</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">{{kelurahans.kelurahan.sekretaris.nomor_telepon}}</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                    <v-divider inset></v-divider>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-email</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">{{kelurahans.kelurahan.sekretaris.email}}</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                </v-list>
                <!-- <v-list v-else>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-account</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">loading</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                    <v-divider inset></v-divider>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-phone</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">loading</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                    <v-divider inset></v-divider>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-email</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">loading</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                </v-list> -->
            </v-card>
        </div>
    </div>
    <transition name="fade" appear>
        <div class="modal-overlay" v-if="showModal" @click="showModal = false"></div>
    </transition>
    <transition name="slide" appear>
          <v-card class="modal" v-if="showModal">
            <v-card-title>
              <span class="text-h5 font-weight-bold">Update Profil Kelurahan</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    md="6"
                    sm="4">
                    
                    <v-text-field
                      v-model="kelurahans.kelurahan.kelurahan"
                      label="Nama Kelurahan"
                      readonly
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="6"
                    sm="4">
                    
                    <v-text-field
                      v-model="kelurahans.kelurahan.id"
                      label="Kode Kelurahan"
                      readonly
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="6"
                    sm="4">
                    
                    <v-text-field
                      v-model="kelurahans.kelurahan.kecamatan"
                      label="Kecamatan"
                      readonly
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="6"
                    sm="4">
                    
                    <v-text-field
                      v-model="kelurahans.kelurahan.kabupaten_kota"
                      label="Kabupaten/Kotar"
                      readonly
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12">
                    
                    <v-text-field
                      v-model="kelurahans.kelurahan.provinsi"
                      label="Provinsi"
                      readonly
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12">

                    <v-text-field
                      v-model="kelurahans.kelurahan.alamat_kantor"
                      label="Alamat Kantor"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                  >
                    <v-text-field
                      v-model="kelurahans.kelurahan.telepon_kelurahan"
                      label="Telepon Kelurahan"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                  >
                    <v-text-field
                      v-model="kelurahans.kelurahan.email_kelurahan"
                      label="Email Kelurahan"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <!-- <v-card-actions> -->
              <v-spacer></v-spacer>
              <v-btn
                color="sik darken-1"
                text
                @click="showModal = false"
              >
                Cancel
              </v-btn>
              <v-btn
                color="sik darken-1"
                text
                @click="handleProfilSubmit(), showModal = false"
              >
                Save
              </v-btn>
            <!-- </v-card-actions> -->
          </v-card>
      </transition>
      <transition name="fade" appear>
        <div class="modal-overlay" v-if="showlurahModal" @click="showlurahModal = false"></div>
    </transition>
    <transition name="slide" appear>
          <v-card class="modal" v-if="showlurahModal">
            <v-card-title>
              <span class="text-h5 font-weight-bold">Update Profil Lurah</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="12">
                    <v-autocomplete
                      v-model="pegawai1"
                      :items="pegawai"
                      clearable
                      item-text="nama"
                      placeholder="Pilih lurah"
                    >
                    </v-autocomplete>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <!-- <v-card-actions> -->
              <v-spacer></v-spacer>
              <v-btn
                color="sik darken-1"
                text
                @click="showlurahModal = false"
              >
                Cancel
              </v-btn>
              <v-btn
                color="sik darken-1"
                text
                @click="handleLurahSubmit(), showlurahModal = false"
              >
                Save
              </v-btn>
            <!-- </v-card-actions> -->
          </v-card>
      </transition>
      <transition name="fade" appear>
        <div class="modal-overlay" v-if="showsekretarisModal" @click="showsekretarisModal = false"></div>
    </transition>
    <transition name="slide" appear>
          <v-card class="modal" v-if="showsekretarisModal">
            <v-card-title>
              <span class="text-h5 font-weight-bold">Update Profil Sekretaris</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="12">
                    <v-autocomplete
                      v-model="pegawai2"
                      :items="pegawai"
                      item-text="nama"
                      placeholder="Pilih Sekretaris"
                    >
                    </v-autocomplete>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <!-- <v-card-actions> -->
              <v-spacer></v-spacer>
              <v-btn
                color="sik darken-1"
                text
                @click="showsekretarisModal = false"
              >
                Cancel
              </v-btn>
              <v-btn
                color="sik darken-1"
                text
                @click="handleSekretarisSubmit(), showsekretarisModal = false"
              >
                Save
              </v-btn>
            <!-- </v-card-actions> -->
          </v-card>
      </transition>
</v-container>
</template>

<script>
import mock from './mock'

export default {
    name: 'Tables',
    data() {
        return {
            select: { nama: '', id: ''},
            showModal: false,
            showlurahModal: false,
            showsekretarisModal: false,
            mock,
            user: JSON.parse(localStorage.getItem('user')),
            pegawai: [],
            kelurahans: null,
            response: null
        }
    },
    
    methods: {
        // async editItem (item) {
        // this.editedIndex = this.kelurahans.indexOf(item)
        // this.editedItem = Object.assign({}, item)
        // this.dialog = true
      // },<th scope="row" class="text-body-1">Alamat Kantor Kelurahan</th>
                                    
        handleProfilSubmit(){
            fetch('http://192.168.0.114:8000/api/kelurahan/update/profil', {
                    method: 'POST',
                    body: JSON.stringify({
                        remember_token: this.user.remember_token,
                        alamat_kantor: this.kelurahans.kelurahan.alamat_kantor,
                        telepon_kelurahan: this.kelurahans.kelurahan.telepon_kelurahan,
                        email_kelurahan: this.kelurahans.kelurahan.email_kelurahan
                    }),
                    headers:{
                    'content-type':'application/json'
                    },
                });
      },
      async handleLurahSubmit(){
        if (this.user){
            let response = await fetch('http://192.168.0.114:8000/api/kelurahan/update/lurah', {
              method: 'POST',
              body: JSON.stringify({
                remember_token: this.user.remember_token,
                nama_lurah: this.pegawai1,
              }),
              headers:{
                'content-type':'application/json'
              },
                });
            response = await response.json();
            this.response = response
            console.log(this.response)
            console.log(this.response.lurah.nama)
            this.kelurahans.kelurahan.lurah.nama = this.response.lurah.nama
            this.kelurahans.kelurahan.lurah.nip = this.response.lurah.nip
            this.kelurahans.kelurahan.lurah.nomor_telepon = this.response.lurah.nomor_telepon
            this.kelurahans.kelurahan.lurah.email = this.response.lurah.email
        }
      },
      async handleSekretarisSubmit(){
        if (this.user){
          let response = await fetch('http://192.168.0.114:8000/api/kelurahan/update/sekretaris', {
            method: 'POST',
            body: JSON.stringify({
              remember_token: this.user.remember_token,
              nama_sekretaris: this.pegawai2,
            }),
            headers:{
              'content-type':'application/json'
            },
          });
          response = await response.json();
          this.response = response
          console.log(this.response)
          console.log(this.response.sekretaris.nama)
          this.kelurahans.kelurahan.sekretaris.nama = this.response.sekretaris.nama
          this.kelurahans.kelurahan.sekretaris.nip = this.response.sekretaris.nip
          this.kelurahans.kelurahan.sekretaris.nomor_telepon = this.response.sekretaris.nomor_telepon
          this.kelurahans.kelurahan.sekretaris.email = this.response.sekretaris.email
        }
      }
    },
    async mounted() {
        if (this.user){
          let response = await fetch('http://192.168.0.114:8000/api/kelurahan/show', {
            method: 'POST',
            body: JSON.stringify({
              remember_token: this.user.remember_token,
            }),
            headers:{
              'content-type':'application/json'
            },
          });
          response = await response.json()
          this.kelurahans = response
        }
            // else (this.$router.push('/dashboard'))

            fetch('http://192.168.0.114:8000/api/kelurahan/perangkat')
            .then(res => res.clone().json())
            .then(res => {
              this.pegawai = res
              console.log(this.pegawai)
            })
            .catch(err => {
              console.log(err)
            })
    // //   const response= fetch('http://192.168.0.122:8000/api/kelurahan',{
    // //       headers:{
    // //           Authorization: 'bearer ' + localStorage.getItem('kelurahan_id')
    // //       }
    // //   })
    // //   console.log(response)
    //         // fetch('http://192.168.0.122:8000/api/kelurahan')
    //         // .then(res => res.json())
    //         // .then(data => this.kelurahans = data)
    //         // .catch(err => console.log(err.message))
    },
    
}
</script>

<style src="./Basic.scss" lang="scss"></style>