<template>
  <v-card>
    
    <v-data-table
      :headers="headers"
      :items="perpenduduk.penduduk"
      sort-by="calories"
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
          <v-dialog
            v-model="dialog"
            
            max-width="500px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                color="sik"
                class="mb-2 white--text"
                v-bind="attrs"
                v-on="on"
                rounded
              >
              <v-icon>mdi-plus</v-icon>
                New Item
              </v-btn>
            </template>
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
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.nama"
                        label="Nama"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.bangunan_id"
                        label="ID Rumah"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        type="date"
                        v-model="editedItem.tanggal_lahir"
                        label="Tanggal Lahir"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.tempat_lahir"
                        label="Tempat Lahir"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.nomor_kk"
                        label="No. KK"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.nik"
                        label="NIK"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.nomor_telepon"
                        label="No. Telp"
                      ></v-text-field>
                    </v-col>
                      <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.email"
                        label="Email"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.jenis_kelamin"
                        label="Jenis Kelamin"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.status_pernikahan"
                        label="Status Pernikahan"
                      ></v-text-field>
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
      <template v-slot:no-data>
        <v-btn
          color="primary"
          @click="headers"
        >
          Reset
        </v-btn>
      </template>
    </v-data-table>
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
        search:'',
        dialog: false,
        dialogDelete: false,
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
        // isEditing: true,
        // model: null,
        editedIndex: -1,
        editedItem: {
          nama: '',
          bangunan_id: 0,
          tanggal_lahir: '',
          tempat_lahir: '',
          nomor_kk: 0,
          nik: 0,
          nomor_telepon: 0,
          email: '',
          jenis_kelamin: '',
          status_pernikahan: '',
        },
        defaultItem: {
          nama: '',
          bangunan_id: 0,
          tanggal_lahir: '',
          tempat_lahir: '',
          nomor_kk: 0,
          nik: 0,
          nomor_telepon: 0,
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
        this.perpenduduk.splice(this.editedIndex, 1)
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

      save () {
        if (this.editedIndex > -1) {
           Object.assign(this.perpenduduk[this.editedIndex], this.editedItem)
           fetch('http://192.168.0.110:8000/api/penduduk/create', {
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
          
         
        } else {
          this.headers.push(this.editedItem)
          console.log(this.editedItem)
          fetch('http://192.168.0.110:8000/api/penduduk/update', {
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
        }
        this.$router.push('/dashboard');
        this.close()
      },
    },

    async mounted() {
        if (this.user){
                let response = await fetch('http://192.168.0.110:8000/api/penduduk/show', {
                    method: 'POST',
                    body: JSON.stringify({
                    remember_token: this.user.remember_token,
                    }),
                    headers:{
                    'content-type':'application/json'
                    },
                });
                
                response = await response.json()
                // console.log(response)
                this.perpenduduk = response
                
            }
            
    //   .then(res => res.json())
    //   .then(data => this.items = data)
    //   .catch(err => console.log(err.message))
            // else (this.$router.push('/dashboard'))
    //   const response= fetch('http://192.168.0.122:8000/api/kelurahan',{
    //       headers:{
    //           Authorization: 'bearer ' + localStorage.getItem('kelurahan_id')
    //       }
    //   })
    //   console.log(response)
            // fetch('http://192.168.0.122:8000/api/kelurahan')
            // .then(res => res.json())
            // .then(data => this.kelurahans = data)
            // .catch(err => console.log(err.message))
    },
  }
</script>
