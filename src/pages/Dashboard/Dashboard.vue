<template>
<v-app :style="{background: $vuetify.theme.themes.light.background}">
    <v-layout row>
        <v-flex md8>
            <v-app :style="{background: $vuetify.theme.themes.dark.background}" class="rounded-tr-xl rounded-br-xl">
                <v-container fluid>
                    <v-row>
                        <v-col cols="12" sm="12">
                            <v-app-bar color="rgba(0,0,0,0)" flat class="mx-8 mb-8 mt-3">
                                <v-text-field prepend-icon="mdi-magnify" color="sik" placeholder="Pencarian Data"></v-text-field>
                                <v-spacer></v-spacer>
                                <v-chip class="pa-2" color="white">
                                    <v-icon left color="rgba(131,129,129,1)">
                                        mdi-calendar
                                    </v-icon>
                                    <v-spacer></v-spacer>
                                    Sabtu, 9 Oktober | 14:02 WITA
                                </v-chip>
                            </v-app-bar>
                        </v-col>
                        <v-col cols="12" sm="12">
                            <v-card class="mx-12 rounded-tl-xl rounded-tr-xl rounded-bl-xl rounded-br-xl mt-n16">
                                <v-list-item three-line>
                                    <v-list-item-content class="">
                                        <v-list-item-title class="headline ">
                                            Hari ini sebanyak <span class="sik--text">-10%</span> warga kelurahan telah<br>
                                            melakukan vaksinasi
                                        </v-list-item-title>
                                        <v-list-item-subtitle>
                                            Kamis, 12 September 2021<br>
                                            terlihat beberapa warga mulai melek akan korontuy <span class="sik--text">#IndonesiaBisa</span> <span class="sik--text"></span>
                                        </v-list-item-subtitle>
                                    </v-list-item-content>

                                    <v-list-item-avatar tile size="190" class="">
                                        <v-img max-height="150" max-width="150" src="https://cdn.pixabay.com/photo/2020/04/29/07/54/coronavirus-5107715_960_720.png"></v-img>
                                    </v-list-item-avatar>
                                </v-list-item>
                            </v-card>
                        </v-col>

                        <v-col cols="12" sm="12">
                            <v-card class="mx-12 rounded-tl-xl rounded-tr-xl rounded-bl-xl rounded-br-xl mt-n9">
                                <v-app-bar color="rgba(0,0,0,0)" flat class="ma-9">
                                    <v-list-item-title class="headline ">Angka Kesembuhan COVID-19 Kelurahan Karame</v-list-item-title>
                                </v-app-bar>
                                <v-btn color="black" text>2020
                                    <v-icon right>
                                        mdi-chevron-down
                                    </v-icon>
                                </v-btn>
                                <v-btn color="rgba(131,129,129,1)" rounded dark depressed>Tahun</v-btn>
                                <v-btn text>Bulan</v-btn>
                                <template>
                                    <v-sparkline :value="value" color="rgba(96,199,101,1)" :smooth="radius || false" :padding="padding" :line-width="width" :stroke-linecap="lineCap" :fill="fill" :type="type" :auto-line-width="autoLineWidth" auto-draw></v-sparkline>
                                </template>
                            </v-card>
                        </v-col>

                        <v-col cols="12" sm="12">
                            <v-card class="mx-12 rounded-tl-xl rounded-tr-xl rounded-bl-xl rounded-br-xl mt-n" color="white">
                                <v-list-item>
                                    <v-list-item-content class="">
                                        <v-list-item-title class="headline black--text font-weight-bold">
                                            Jumlah Penduduk <br>
                                            <h1></h1>
                                            <h3 class="font-weight-light">{{ data.jumlah_penduduk }} Jiwa</h3>
                                        </v-list-item-title>
                                    </v-list-item-content>
                                    <v-list-item-avatar tile size="140" class="">
                                        <v-icon size="100" color="sik">mdi-human-male-female-child</v-icon>
                                    </v-list-item-avatar>

                                </v-list-item>
                            </v-card>
                        </v-col>

                        <v-col cols="12" sm="12">
                            <v-card class="mx-12 rounded-tl-xl rounded-tr-xl rounded-bl-xl rounded-br-xl mt-n1" color="white">
                                <v-list-item three-line dense>
                                    <v-list-item-content class="">
                                        <v-list-item-title class="headline black--text font-weight-bold">
                                            Jumlah Bangunan <br>
                                            <h1></h1>
                                            <h3 class="font-weight-light">{{ data.jumlah_bangunan }} Bangunan</h3>
                                        </v-list-item-title>
                                    </v-list-item-content>

                                    <v-list-item-avatar tile size="140" class="">
                                        <v-icon size="100" color="sik">mdi-home-city</v-icon>
                                    </v-list-item-avatar>
                                </v-list-item>
                            </v-card>
                        </v-col>

                        <!-- <v-col cols="12" sm="6">
                            <v-card class="mx-12 rounded-tl-xl rounded-tr-xl rounded-bl-xl rounded-br-xl mt-n10">
                                <v-app-bar color="rgba(0,0,0,0)" flat class="ma-8">
                                    <h5><br>Aktivitas</h5>
                                    <v-spacer></v-spacer>
                                    <v-btn color="sik" rounded dark depressed>Minggu</v-btn>
                                    <v-btn text>Bulan</v-btn>
                                </v-app-bar>
                                <v-progress-circular rotate="360" size="100" width="15" value="50" color="#E53C38" class="mt-n5 ml-5 mb-2">
                                    50
                                </v-progress-circular>
                                <v-progress-circular rotate="360" size="100" width="15" value="70" color="#D11D27" class="mt-n5 ml-2 mb-2">
                                    70
                                </v-progress-circular>
                                <v-progress-circular rotate="360" size="100" width="15" value="90" color="#A90011" class="mt-n5 ml-2 mb-2">
                                    90
                                </v-progress-circular>
                            </v-card>
                        </v-col> -->
                    </v-row>
                </v-container>
            </v-app>
        </v-flex>
        <v-flex md4>
            <v-app :style="{background: $vuetify.theme.themes.light.background}">
                <v-container fluid>
                    <v-row>
                        <v-col cols="12" md="12">
                            <v-card class="mt-16 mr-12  rounded-tl-xl rounded-tr-xl rounded-bl-xl rounded-br-xl">
                                <v-app-bar color="rgba(0,0,0,0)" flat class="ma-6">
                                    <h5 class="mt-5"><br>Penduduk Berdasarkan<br> Kelamin</h5>
                                    <v-spacer></v-spacer>
                                    <!-- <v-btn color="rgba(131,129,129,1)" rounded dark depressed>Minggu</v-btn>
                                    <v-btn text>Bulan</v-btn> -->
                                </v-app-bar>
                                <div>
                                    <v-row>
                                        <v-col cols="6">
                                <v-list-item-avatar tile size="70" class="">
                                    <v-icon size="70" color="rgba(93,173,241,1)">mdi-human-male</v-icon>
                                </v-list-item-avatar>
                                <v-progress-circular rotate="360" size="70" width="15" :value="data.presentase_pria" color="rgba(93,173,241,1)" class="">
                                    {{ data.jumlah_pria }}
                                </v-progress-circular>
                                
                                 </v-col>
                                 <v-col cols="6">
                           
                                <!-- <v-divider vertical></v-divider> -->
                              
                                <v-list-item-avatar tile size="70" class="">
                                    <v-icon size="70" color="rgba(241,93,137,1)">mdi-human-female</v-icon>
                                </v-list-item-avatar>
                                <v-progress-circular rotate="360" size="70" width="15" :value="data.presentase_wanita" color="rgba(241,93,137,1)" class="">
                                    {{ data.jumlah_wanita }}
                                </v-progress-circular>
                                        </v-col>
                                    </v-row>
                                </div>
                            </v-card>

                            <v-card class="mt-6 mr-12 rounded-tl-xl rounded-tr-xl rounded-bl-xl rounded-br-xl">
                                <v-list-item two-line>
                                    <v-list-item-content>
                                        <v-list-item-title class="text-h5 pt-5 pl-4">
                                            Manado
                                        </v-list-item-title>
                                        <v-list-item-subtitle class="pl-4">Minggu, 10 Oktober | 12:30 WITA | Cerah Berawan</v-list-item-subtitle>
                                    </v-list-item-content>
                                </v-list-item>

                                <v-card-text>
                                    <v-row align="center" class="pl-4">
                                        <v-col class="text-h2" cols="6">
                                            26&deg;C
                                        </v-col>
                                        <v-col cols="6">
                                            <v-img src="https://cdn.vuetifyjs.com/images/cards/sun.png" alt="Sunny image" width="92"></v-img>
                                        </v-col>
                                    </v-row>
                                </v-card-text>

                                <v-list-item>
                                    <v-list-item-icon class="pl-4">
                                        <v-icon>mdi-send</v-icon>
                                    </v-list-item-icon>
                                    <v-list-item-subtitle>23 km/h</v-list-item-subtitle>
                                </v-list-item>

                                <v-list-item>
                                    <v-list-item-icon class="pl-4">
                                        <v-icon>mdi-cloud</v-icon>
                                    </v-list-item-icon>
                                    <v-list-item-subtitle>48%</v-list-item-subtitle>
                                </v-list-item>

                                <v-slider v-model="time" :max="6" :tick-labels="labels" class="mx-4" ticks></v-slider>

                                <v-list class="transparent">
                                    <v-list-item v-for="item in forecast" :key="item.day">
                                        <v-list-item-title>{{ item.day }}</v-list-item-title>

                                        <v-list-item-icon>
                                            <v-icon>{{ item.icon }}</v-icon>
                                        </v-list-item-icon>

                                        <v-list-item-subtitle class="text-right">
                                            {{ item.temp }}
                                        </v-list-item-subtitle>
                                    </v-list-item>
                                </v-list>
                            </v-card>
                        </v-col>
                    </v-row>
                </v-container>
            </v-app>
        </v-flex>

        <template>
            <v-carousel class=" mt-2 rounded-tl-xl rounded-tr-xl rounded-bl-xl rounded-br-xl">
                <v-carousel-item
                v-for="(item,i) in items"
                :key="i"
                :src="item.src"
                reverse-transition="fade-transition"
                transition="fade-transition"
                ></v-carousel-item>
            </v-carousel>
        </template>

    </v-layout>
</v-app>
</template>

<script>
export default {
    data: () => ({
        model: 0,
        items: [
          {
            src: 'https://1.bp.blogspot.com/-cXpLeIY5o60/YMOGFiyX0SI/AAAAAAAAFQE/GCucuxSpWmM5HgOSerEeml9OQe7C2GhOACLcBGAsYHQ/s2048/20210611_133414.jpg',
          },
          {
            src: 'https://detikmanado.com/wp-content/uploads/2019/05/ketua-PMI-Manado.jpg',
          },
          {
            src: 'https://i.ytimg.com/vi/-MvvZnfeVQg/maxresdefault.jpg',
          },
          {
            src: 'https://cdn-2.tstatic.net/manado/foto/bank/images2/andrei-angouw-6576gfhgfhgh.jpg',
          },
        ],
        data: [],
        width: 2,
        radius: 10,
        padding: 8,
        lineCap: 'round',
        value: [0, 2, 5, 9, 5, 10, 3, 5, 0, 0, 1, 8, 2, 9, 0],
        fill: false,
        type: 'trend',
        autoLineWidth: false,
        labels: ['MIN', 'SEN', 'SEL', 'RAB', 'KAM', 'JUM', 'SAB'],
        time: 0,
        user: JSON.parse(localStorage.getItem('user')),
        forecast: [{
                day: 'Senin',
                icon: 'mdi-white-balance-sunny',
                temp: '24\xB0/12\xB0'
            },
            {
                day: 'Selasa',
                icon: 'mdi-white-balance-sunny',
                temp: '22\xB0/14\xB0'
            },
            {
                day: 'Rabu',
                icon: 'mdi-cloud',
                temp: '25\xB0/15\xB0'
            },

        ],
    }),
    computed: {
        theme() {
            return this.$vuetify.theme.dark ? "dark" : "light";
        }

    },

    async mounted() {
        if (this.user){
                let response = await fetch('http://192.168.1.20:8000/api/dashboard', {
                    method: 'POST',
                    body: JSON.stringify({
                        remember_token: this.user.remember_token,
                    }),
                    headers:{
                    'content-type':'application/json'
                    },
                });
                response = await response.json()
                this.data = response
                console.log(this.data)
            }
            // else (this.$router.push('/dashboard'))
        }
}
</script>

<style src="./Dashboard.scss" lang="scss" scoped/>
