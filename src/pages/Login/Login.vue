<template>
  <v-app>
    <v-container fluid>
      <v-row no-gutters>
        <v-col cols="7" class="main-part d-none d-md-none d-lg-flex">
          <div class="d-flex">
            <v-img src="@/assets/logosik.png" max-width="750"></v-img>
            <!-- <p>SISTEM INFORMASI KELURAHAN</p> -->
          </div>
        </v-col>
        <v-col cols="12" lg="5" class="login-part d-flex align-center justify-center">
          <v-row no-gutters class="align-start">
            <v-col cols="12" class="login-part d-flex align-center justify-center flex-column">
              <div class="login-wrapper pt-md-4 pt-0">
                <v-tabs grow color="sik">
                  <v-tabs-slider></v-tabs-slider>
                  <v-tab :href="`#tab-login`">
                    <b>HALAMAN MASUK</b>
                  </v-tab>
                  <!-- <v-tab :href="`#tab-newUser`">
                    New User
                  </v-tab> -->

                  <v-tab-item :value="'tab-login'" >
                    <v-form>
                      <v-container>
                        <v-row class="flex-column">
                          <v-col>
                            <p class="login-slogan display-2 text-center font-weight-medium my-10">Selamat Datang di SIK</p>
                            <!-- <v-btn height="45" block color="white" elevation="0" class="google text-capitalize">
                              <v-img src="@/assets/google.svg" max-width="30" class="mr-4"></v-img>
                              Sign in with Google</v-btn> -->
                          </v-col>
                          <!-- <v-col cols="12" class="d-flex align-center my-8">
                            <v-divider></v-divider>
                            <span class="px-5"> or </span>
                            <v-divider></v-divider>
                          </v-col> -->
                          <v-form @submit="login()">
                            <v-col>
                              <v-text-field
                                  v-model="email"
                                  label="Email Address"
                                  :rules="emailRules"
                                  type="email"
                                  required
                              ></v-text-field>
                              <v-text-field
                                  v-model="password"
                                  :rules="passRules"
                                  type="password"
                                  label="Password"
                                  hint="At least 5 characters"
                                  required
                              ></v-text-field>

                            </v-col>
                            <v-col class="d-flex justify-space-between">
                              <v-btn
                                  type="submit"
                                  block
                                  class="text-capitalize"
                                  large
                                  outlined
                                  :loading="loading"
                                  :disabled="password.length < 5 || email.length === 0"
                                  color="sik"
                                  @keyup.native.enter="login"
                              >
                                MASUK</v-btn>
                              <!-- <v-btn large text class="text-capitalize primary--text">Forget Password</v-btn> -->
                            </v-col>
                          </v-form>
                        </v-row>
                      </v-container>
                    </v-form>
                  </v-tab-item>

                  <!-- <v-tab-item :value="'tab-newUser'" >
                    <v-form>
                      <v-container>
                        <v-row class="flex-column">

                          <v-col>
                            <p class="login-slogan display-2 text-center font-weight-medium mt-10">Welcome!</p>
                            <p class="login-slogan display-1 text-center font-weight-medium mb-10">Create your account</p>
                          </v-col>

                          <v-form>
                            <v-col>
                              <v-text-field
                                  v-model="createFullName"
                                  label="Full Name"
                                  required
                              ></v-text-field>
                              <v-text-field
                                  v-model="createEmail"
                                  :rules="emailRules"
                                  label="Email Address"
                                  required
                              ></v-text-field>
                              <v-text-field
                                  v-model="createPassword"
                                  :rules="passRules"
                                  type="password"
                                  label="Password"
                                  hint="At least 6 characters"
                                  required
                              ></v-text-field>
                            </v-col>
                            <v-col class="d-flex justify-space-between">
                              <v-btn
                                  large
                                  block
                                  :disabled="createFullName.length === 0 || createEmail.length === 0 || createPassword === 0"
                                  color="primary"
                                  @click="login"
                              >
                                Create your account</v-btn>
                            </v-col>
                          </v-form>

                          <v-col cols="12" class="d-flex align-center my-4">
                            <v-divider></v-divider>
                            <span class="px-5"> or </span>
                            <v-divider></v-divider>
                          </v-col>

                          <v-btn height="45" block color="white" elevation="0" class="google text-capitalize">
                            <v-img src="@/assets/google.svg" max-width="30" class="mr-4"></v-img>
                            Sign in with Google</v-btn>
                        </v-row>
                      </v-container>
                    </v-form>
                  </v-tab-item> -->

                </v-tabs>
              </div>
            </v-col>
            <v-col cols="12" class="d-flex justify-center">
              <v-footer>
                <div class="sik--text">© 2021 <a href="https://flatlogic.com/" class="text-decoration-none sik--text"><b>UPTD PDIG BAPELITBANGDA</b></a>, LLC. All rights reserved.</div>
              </v-footer>
            </v-col>
          </v-row>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>

let loginApi = 'http://192.168.1.20:8000/api'

  export default {
    name: 'Login',
    data() {
      return {
        email: '',
        emailRules: [
          v => !!v || 'E-mail is required',
          v => /.+@.+/.test(v) || 'E-mail must be valid',
        ],
        password: '',
        passRules: [
          v => !!v || 'Password is required',
          v => v.length >= 5 || 'Min 5 characters'
        ],
        user: JSON.parse(localStorage.getItem('user')),
        loading: false
      }
    },
    
    methods: {
      async login(){
        this.loading= true
        try {
          let response = await fetch(loginApi+'/login', {
          method: 'POST',
            body: JSON.stringify({
              email: this.email,
              password: this.password,
            }),
            headers:{
              'content-type':'application/json'
            },
          });
          response = await response.json();
          window['response']= response;       
          window.localStorage.setItem('user', JSON.stringify(
            response.user
          ))
          this.$router.push('/dashboard');
        } 
        catch (error) {
          
          console.error(error);
          alert('Email dan Password salah')
        }
        this.loading= false
        
        
        
        // if (!response.ok) {
        // let message = `An error has occured: ${response.status}`;
        // throw new Error(message);
        // }
        // console(data);
        // waits until the request completes...
        
      },


      // }
      // loginBadStatus().catch(error => {
      // error.message; // 'An error has occurred: 404'
      // });

      // login().then(data => {data;})
    },
    mounted(){
      console.log(this.user)
        if (this.user != null){
          this.$router.push('/dashboard')
        }
    }
        
    
}
  
    
  

</script>

<style src="./Login.scss" lang="scss"/>
