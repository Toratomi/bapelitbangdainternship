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
                                <v-btn icon v-bind="attrs" v-on="on">
                                    <v-icon color="grey">mdi-dots-vertical</v-icon>
                                </v-btn>
                            </template>
                            <v-list>
                                <v-list-item v-for="(item, i) in mock.menu" :key="i" @click="() => {}">
                                    <v-list-item-title>{{ item }}</v-list-item-title>
                                </v-list-item>
                            </v-list>
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
                                    <td class="text-body-1 black--text font-weight-light"><span id="namakelurahan">Karame</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Kode Kelurahan</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="kodekelurahan">002</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Kecamatan</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="kecamatan">Singkil</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Kabupaten/Kota</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="kabupaten">Manado</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Provinsi</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="provinsi">Sulawesi Utara</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Alamat Kantor Kelurahan</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="alamatkantorkelurahan">Jl. Teuku Umar No.1</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Telepon Kelurahan</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="teleponkelurahan">(0431) 816086</span></td>
                                </tr>
                                <tr>
                                    <th scope="row" class="text-body-1">Email Kelurahan</th>
                                    <td class="text-body-1 black--text font-weight-light"><span id="emailkelurahan">karamemanadokota@gmail.com</span></td>
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
                            <v-btn icon v-bind="attrs" v-on="on">
                                <v-icon color="grey">mdi-dots-vertical</v-icon>
                            </v-btn>
                        </template>
                        <v-list>
                            <v-list-item v-for="(item, i) in mock.menu" :key="i" @click="() => {}">
                                <v-list-item-title>{{ item }}</v-list-item-title>
                            </v-list-item>
                        </v-list>
                    </v-menu>
                </v-card-title>
                <v-list>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-account</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">Nurhadi Aldo</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                    <v-divider inset></v-divider>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-phone</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">(0431) 816086</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                    <v-divider inset></v-divider>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-email</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">nurhadialdo@gmail.com</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                </v-list>
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
                            <v-btn icon v-bind="attrs" v-on="on">
                                <v-icon color="grey">mdi-dots-vertical</v-icon>
                            </v-btn>
                        </template>
                        <v-list>
                            <v-list-item v-for="(item, i) in mock.menu" :key="i" @click="() => {}">
                                <v-list-item-title>{{ item }}</v-list-item-title>
                            </v-list-item>
                        </v-list>
                    </v-menu>
                </v-card-title>
                <v-list>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-account</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">Merlin Blonteng</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                    <v-divider inset></v-divider>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-phone</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">(0431) 816086</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                    <v-divider inset></v-divider>
                    <v-list-item>
                        <v-list-item-action>
                            <v-icon>mdi-email</v-icon>
                        </v-list-item-action>
                        <v-list-item-content>
                            <v-list-item-title class="text-h9">susicantik@gmail.com</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                </v-list>
            </v-card>
        </div>
    </div>
       
</v-container>
</template>

<script>
import mock from './mock'

export default {
    name: 'Tables',
    data() {
        return {
            mock
        }
    }
}
</script>

<style src="./Basic.scss" lang="scss"></style>