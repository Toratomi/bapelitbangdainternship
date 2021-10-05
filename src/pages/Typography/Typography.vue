<template>
  <v-card>
    <v-data-table
      :search="search"
      :headers="headers"
      :items="perpenduduk.penduduk"
      :loading="isLoading"
      loading-text="Loading... Please wait"
      sort-by="penduduk"
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar
          flat
        >
          <v-toolbar-title class="text-h6 black--text font-weight-black">Pencarian Data Penduduk</v-toolbar-title>
          <v-divider
            class="mx-4"
            inset
            vertical
          ></v-divider> 
            <v-text-field
              v-model="search"
              append-icon="mdi-magnify"
              label="Search"
              single-line
              hide-details
            ></v-text-field>
            <template >
              <v-btn
                color="sik"
                class="mb-2 white--text"
                rounded 
                @click="showaddModal = true"
              >
              <v-icon>mdi-plus</v-icon>
                Tambah Penduduk
              </v-btn>
            </template>
          <v-dialog
            v-model="dialog"
            
            max-width="500px"
          >
          
            <v-card>
              <v-card-title>
                <span class="text-h5">{{ formTitle }}</span>
              </v-card-title>

              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        v-model="editedItem.nama"
                        label="Nama"
                        onkeypress="return (event.charCode > 64 && event.charCode < 91) || (event.charCode > 96 && event.charCode < 123) || (event.charCode==32)"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-spacer></v-spacer>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    ><v-autocomplete
                      v-model="editedItem.bangunan_id"
                      :items="perpenduduk.bangunan_id"
                      item-text="id"
                      item-value="id"
                      placeholder="ID Rumah"
                      type="number"
                      required
                    >
                    </v-autocomplete>
                      <!-- <v-text-field
                        v-model="editedItem.bangunan_id"
                        label="ID Rumah"
                      ></v-text-field> -->
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        type="date"
                        v-model="editedItem.tanggal_lahir"
                        label="Tanggal Lahir"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        v-model="editedItem.tempat_lahir"
                        label="Tempat Lahir"
                        onkeypress="return (event.charCode > 64 && event.charCode < 91) || (event.charCode > 96 && event.charCode < 123) || (event.charCode==32)"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        v-model="editedItem.nomor_kk"
                        label="No. KK"
                        type="number"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-spacer></v-spacer>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        v-model="editedItem.nik"
                        label="NIK"
                        type="number"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        v-model="editedItem.nomor_telepon"
                        label="No. Telp"
                        type="number"
                        required
                      ></v-text-field>
                    </v-col>
                      <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        v-model="editedItem.email"
                        label="Email"
                        type="email"
                        :rules="emailRules"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="5"
                    ><v-autocomplete
                      v-model="editedItem.jenis_kelamin"
                      :items="gender"
                      :search-input="search"
                      item-text="nama"
                      item-value="nama"
                      placeholder="Jenis Kelamin"
                      required
                    >
                    </v-autocomplete>
                      <!-- <v-text-field
                        v-model="editedItem.jenis_kelamin"
                        label="Jenis Kelamin"
                      ></v-text-field> -->
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="7"
                    ><v-autocomplete
                      v-model="editedItem.status_pernikahan"
                      :items="status"
                      item-text="nama"
                      item-value="nama"
                      placeholder="Status Pernikahan"
                      required 
                    >
                    </v-autocomplete>
                      <!-- <v-text-field
                        v-model="editedItem.status_pernikahan"
                        label="Status Pernikahan"
                      ></v-text-field> -->
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  color="sik darken-1"
                  text
                  @click="close"
                >
                  Cancel
                </v-btn>
                <v-btn
                  color="sik darken-1"
                  text
                  @click="save"
                >
                  Save
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="text-h5">Apa anda yakin akan menghapus item ini?</v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="sik" text @click="closeDelete">Cancel</v-btn>
                <v-btn color="sik darken-1" text @click="deleteItemConfirm">OK</v-btn>
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:[`item.actions`]="{ item }">
        <v-icon
          small
          class="mr-2"
          @click="editItem(item)"
        >
          mdi-pencil
        </v-icon>
        <v-icon
          small
          @click="deleteItem(item)"
        >
          mdi-delete
        </v-icon>
      </template>
      
    </v-data-table>
    <transition name="fade" appear>
      <div class="modal-overlay" v-if="showaddModal" @click="showaddModal = false"></div>
    </transition>
      <transition name="slide" appear>    
        <v-card class="modal" v-if="showaddModal" max-width="500px">    
          <v-card-title>
            <span class="text-h5">Tambah Penduduk</span>
          </v-card-title>    

              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        v-model="editedItem.nama"
                        label="Nama"
                        onkeypress="return (event.charCode > 64 && event.charCode < 91) || (event.charCode > 96 && event.charCode < 123) || (event.charCode==32)"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-spacer></v-spacer>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    ><v-autocomplete
                      v-model="editedItem.bangunan_id"
                      :items="perpenduduk.bangunan_id"
                      item-text="id"
                      item-value="id"
                      placeholder="ID Rumah"
                      type="number"
                      required
                    >
                    </v-autocomplete>
                      <!-- <v-text-field
                        v-model="editedItem.bangunan_id"
                        label="ID Rumah"
                      ></v-text-field> -->
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        type="date"
                        v-model="editedItem.tanggal_lahir"
                        label="Tanggal Lahir"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        v-model="editedItem.tempat_lahir"
                        label="Tempat Lahir"
                        onkeypress="return (event.charCode > 64 && event.charCode < 91) || (event.charCode > 96 && event.charCode < 123) || (event.charCode==32)"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        v-model="editedItem.nomor_kk"
                        label="No. KK"
                        type="number"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-spacer></v-spacer>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        v-model="editedItem.nik"
                        label="NIK"
                        type="number"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        v-model="editedItem.nomor_telepon"
                        label="No. Telp"
                        type="number"
                        required
                      ></v-text-field>
                    </v-col>
                      <v-col
                      cols="12"
                      sm="6"
                      md="6"
                    >
                      <v-text-field
                        v-model="editedItem.email"
                        label="Email"
                        type="email"
                        :rules="emailRules"
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="5"
                    ><v-autocomplete
                      v-model="editedItem.jenis_kelamin"
                      :items="gender"
                      :search-input="search"
                      item-text="nama"
                      item-value="nama"
                      placeholder="Jenis Kelamin"
                      required
                    >
                    </v-autocomplete>
                      <!-- <v-text-field
                        v-model="editedItem.jenis_kelamin"
                        label="Jenis Kelamin"
                      ></v-text-field> -->
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="7"
                    ><v-autocomplete
                      v-model="editedItem.status_pernikahan"
                      :items="status"
                      item-text="nama"
                      item-value="nama"
                      placeholder="Status Pernikahan"
                      required 
                    >
                    </v-autocomplete>
                      <!-- <v-text-field
                        v-model="editedItem.status_pernikahan"
                        label="Status Pernikahan"
                      ></v-text-field> -->
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  color="sik darken-1"
                  text
                  @click="showaddModal=false"
                >
                  Batal
                </v-btn>
                <v-btn
                  color="sik darken-1"
                  text
                  @click="verifikasiadd=true"
                >
                  Tambah
                </v-btn>
              </v-card-actions>
              
            </v-card>
          </transition>
          <transition name="slide" appear>    
          <v-card class="modal" v-if="verifikasiadd" max-width="800px">    
            <v-card-title>
              <span class="text-h5">Apakah benar akan menambahkan <strong>{{editedItem.nama}} </strong> pada data penduduk?</span>
            </v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  color="sik darken-1"
                  text
                  @click="verifikasiadd=false"
                >
                  Batal
                </v-btn>
                <v-btn
                  color="sik darken-1"
                  text
                  @click="verifikasiadd=false, showaddModal=false, add()"
                >
                  Konfirmasi
                </v-btn>
              </v-card-actions>
            </v-card>
          </transition>
  </v-card>
  
</template>
<!-- <v-card>
    <v-card-title>
      <v-autocomplete
          v-model="headers"
          append-icon="mdi-magnify"
          :label="Search"
          :items="perpenduduk.penduduk.nama"
          hide-details
        >
      </v-autocomplete>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="perpenduduk.penduduk"
      :search="search"
      items-per-page="5"
    ></v-data-table>
  </v-card>
</template> -->

<script>
  export default {
    data () {
      return {
        emailRules: [
          v => !!v || 'E-mail is required',
          v => /.+@.+/.test(v) || 'E-mail must be valid',
        ],
        isLoading: false,
        search:'',
        showaddModal: false,
        dialog: false,
        dialogDelete: false,
        verifikasiadd: false,
        verifikasiedit: false,
        user: JSON.parse(localStorage.getItem('user')),
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
          { text: 'Actions', value: 'actions', sortable: false },
          
        ],
        perpenduduk: [],
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
        defaultItem: {
          nama: '',
          bangunan_id: 0,
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
    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
    },
     watch: {
      dialog (val) {
        val || this.close()
      },
      dialogDelete (val) {
        val || this.closeDelete()
      },
    },
    // mounted() {
    //   fetch('http://192.168.0.110:8000/api/penduduk/show')
    //   .then(res => res.json())
    //   .then(data => this.items = data)
    //   .catch(err => console.log(err.message))
    // },
    methods: {
      editItem (penduduk) {
        this.editedIndex = this.perpenduduk
        this.editedItem = Object.assign({}, penduduk)
        
        
        this.dialog = true
      },

      deleteItem (penduduk) {
        this.editedIndex = this.perpenduduk
        this.editedItem = Object.assign({}, penduduk)
        this.dialogDelete = true
      },

      deleteItemConfirm () {
        fetch('http://192.168.1.70:8000/api/penduduk/delete', {
                    method: 'POST',
                    body: JSON.stringify({
                        remember_token: this.user.remember_token,
                        id: this.editedItem.id,
                    }),
                    headers:{
                    'content-type':'application/json'
                    },
                    
                });
        this.closeDelete()
      },

      close () {
        this.dialog = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      closeDelete () {
        this.dialogDelete = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      add() {
        console.log(this.editedItem)
        fetch('http://192.168.1.70:8000/api/penduduk/create', {
                    method: 'POST',
                    body: JSON.stringify({
                        remember_token: this.user.remember_token,
                        nama: this.editedItem.nama,
                        bangunan_id: this.editedItem.bangunan_id,
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
      },

      save () {
        if (this.editedIndex > -1) {
          //  Object.assign(this.perpenduduk[this.editedIndex], this.editedItem)
          //  fetch('http://192.168.1.60:8000:8000/api/penduduk/upadate', {
          //           method: 'POST',
          //           body: JSON.stringify({
          //               remember_token: this.user.remember_token,
          //               id: this.editedItem.id,
          //               nama: this.editedItem.nama,
          //               bangunan_id: this.editedItem.bangunan_id,
          //               tanggal_lahir: this.editedItem.tanggal_lahir,
          //               tempat_lahir: this.editedItem.tempat_lahir,
          //               nomor_kk: this.editedItem.nomor_kk,
          //               nik: this.editedItem.nik,
          //               nomor_telepon: this.editedItem.nomor_telepon,
          //               email: this.editedItem.email,
          //               jenis_kelamin: this.editedItem.jenis_kelamin,
          //               status_pernikahan: this.editedItem.status_pernikahan,

          //           }),
          //           headers:{
          //           'content-type':'application/json'
          //           },
                    
          //       });
          
         
        } else {
            this.headers.push(this.editedItem)
            console.log(this.editedItem)
            fetch('http://192.168.1.70:8000/api/penduduk/update', {
                      method: 'POST',
                      body: JSON.stringify({
                          remember_token: this.user.remember_token,
                          id: this.editedItem.id,
                          nama: this.editedItem.nama,
                          bangunan_id: this.editedItem.bangunan_id,
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
                  console.log(this.perpenduduk.penduduk.nama)
        }
        // this.$router.push('/dashboard');
            
        this.close()
      },
    },

    async mounted() {
        this.isLoading = true
        if (this.user.role === 'operator'){
                let response = await fetch('http://192.168.1.70:8000/api/penduduk/show', {
                    method: 'POST',
                    body: JSON.stringify({
                    remember_token: this.user.remember_token,
                    kelurahan_id: this.kelurahan_id
                    }),
                    headers:{
                    'content-type':'application/json'
                    },
                });
                
                response = await response.json()
                // console.log(response)
                this.perpenduduk = response
                this.isLoading = false
                
            }
            
    //   .then(res => res.json())
    //   .then(data => this.items = data)
    //   .catch(err => console.log(err.message))
            else (this.$router.push('/dashboard'))
    //   const response= fetch('http://192.168.0.122:8000/api/kelurahan',{
    //       headers:{
    //           Authorization: 'bearer ' + localStorage.getItem('kelurahan_id')
    //       }
    //   })
      // console.log(response)
            
            // fetch('http://192.168.1.60:8000/api/penduduk/show',  {
            //         method: 'POST',
            //         body: JSON.stringify({
            //         remember_token: this.user.remember_token,
            //         }),
            //         headers:{
            //         'content-type':'application/json'
            //         },
            //     })
                
            // .then(res => res.json())
            // .then(data => this.perpenduduk = data)
            // .catch(err => console.log(err.message))
            // .finally(() => (this.isLoading = false))
            // console.log(this.perpenduduk)
    },
  }
</script>

<style src="./Typography.scss" lang="scss"></style>
