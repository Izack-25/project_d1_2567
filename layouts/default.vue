<template>
  <v-app>
    <v-card>
      <v-layout>
      
        <v-app-bar
          color="teal-darken-4"
          image="https://img.freepik.com/free-photo/calathea-orbifolia-natural-leaves-background_53876-98363.jpg?w=1060&t=st=1722325655~exp=1722326255~hmac=ac284cc50a80bfdecf194fa8442a24fae63c4b5d566e5e994aa3e250dc1baa91">
          <v-app-bar-title>My account</v-app-bar-title>
        </v-app-bar>

        <v-navigation-drawer 
          class="bg-black"
          v-model="drawer"
          :rail="rail"
          permanent
          @click="rail = false"
        >
        
          <v-list-item 
            prepend-avatar="\assets\images\Dum.jpg"
            title="My Profile"
            nav
          >
          
            <template v-slot:append>
              <v-btn
                icon="mdi-chevron-left"
                variant="text"
                @click.stop="rail = !rail"
              ></v-btn>
            </template>
          </v-list-item>
          <v-divider></v-divider>
          <v-list density="compact" nav>
            <v-list-item
              prepend-icon="mdi-home-city"
              title="หน้าหลัก"
              value="home"
              @click="main"
            ></v-list-item>
            <v-list-item
           
              prepend-icon="mdi-plus"
              size="x-large"
              title="เพิ่มข้อมูล"
              value="add"
              @click="add"
            ></v-list-item>
            <v-list-item
              prepend-icon="mdi-home-city"
              title="แสดงข้อมูล"
              value="home"
              @click="showdata"
            ></v-list-item>
            <v-list-item
              prepend-icon="mdi-account"
              title="เกี่ยวกับ"
              value="account"
              @click="about"
            ></v-list-item>
            <v-list-item
              prepend-icon="mdi-close-circle"
              title="LOGOUT"
              value="users"
              @click="logOut"
            ></v-list-item>
          </v-list>
        </v-navigation-drawer>
        <!-- <v-main style="height: 650px"></v-main> -->
        <v-main >
         <NuxtPage></NuxtPage>
          <!-- Main Content -->
        </v-main>
        <!--  -->
      </v-layout>
    </v-card>
  </v-app>
</template>
<script>
import axios from 'axios';
export default {
  data() {
    return {
      drawer: true,
      rail: true,
      username: '',
      email: '',
    };
  },
  async mounted() {
    let user = window.sessionStorage.getItem("email");
    console.log("user=", user);
    if (!user) {
      this.$router.replace("/login");
      return
    }
  },
  methods: {
    main(){
      this.$router.replace("/home");
    },
    listdata(){
      this.$router.replace("/listdata");
    },
    showdata(){
      this.$router.replace("/showdata");
    },
    about(){
      this.$router.replace("/about");
    },
    add(){
      this.$router.replace("/add");
    },
    logOut() {
      window.sessionStorage.clear();
      this.$router.replace("/login");
    },
  },
};
</script>

