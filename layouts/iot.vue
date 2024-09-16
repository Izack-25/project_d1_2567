<template>
    <v-layout class="rounded rounded-md">
      <v-navigation-drawer>
        <v-list>

            <template>
  <v-card class="mx-auto" color="grey-lighten-3" max-width="448">
    <v-layout>
      <v-main>
        <v-container fluid>
          <v-row dense>
            <v-col
              v-for="n in 4"
              :key="n"
              cols="12"
            >
              <v-card
                :subtitle="`Subtitle for Content ${n}`"
                :title="`Content ${n}`"
                text="Lorem ipsum dolor sit amet consectetur, adipisicing elit.?"
              ></v-card>
            </v-col>
          </v-row>
        </v-container>
      </v-main>
    </v-layout>
  </v-card>
</template>

        </v-list>
      </v-navigation-drawer>
  
      <v-navigation-drawer location="right">
        <v-list>
          <v-list-item title="Drawer right"></v-list-item>
        </v-list>
      </v-navigation-drawer>
  
      <v-main class="d-flex align-center justify-center" style="min-height: 300px;">
        <NuxtPage />
        <v-app-bar
          color="teal-darken-4"
          image="https://img.freepik.com/free-photo/calathea-orbifolia-natural-leaves-background_53876-98363.jpg?w=1060&t=st=1722325655~exp=1722326255~hmac=ac284cc50a80bfdecf194fa8442a24fae63c4b5d566e5e994aa3e250dc1baa91">
          <v-app-bar-title>My account</v-app-bar-title>
        </v-app-bar>
      </v-main>
    </v-layout>
  </template>