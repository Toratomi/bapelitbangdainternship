<template>
    <div>
    <v-app class="pa-6"
    >
        <Header
        :loading="loading"/>
        <Sidebar
        :loading="loading"/>
        <v-main class="content">
            <router-view />
            <Footer />
        </v-main>
    </v-app>
    </div>
</template>


<script>
    import Header from '@/components/Header/Header';
    import Sidebar from '@/components/Sidebar/Sidebar';
    import Footer from "@/components/Footer/Footer";
    import './Layout.scss';
// import { use } from 'vue/types/umd';

    export default {
        name: 'Layout',
        data() {
            return {
                loading: false,
                user: JSON.parse(localStorage.getItem('user'))
            }
        },
        components: {Footer, Header, Sidebar },
        async mounted(){
            this.loading= true
            try {
                if (this.user){
                    let response = await fetch('http://192.168.0.121:8000/api/auth', {
                        method: 'POST',
                        body: JSON.stringify({
                            token: this.user.remember_token,
                            page: this.user.id
                        }),
                        headers:{
                        'content-type':'application/json'
                        },
                    });
                    response = await response.json()
                    console.log(response)
                }
            } catch (error) {
                this.$router.push('/login')
                alert('Terjadi Kesalahan Mohon Login Kembali')
            }
            this.loading= false
        }
    };
</script>

<style src="./Layout.scss" lang="scss" />
