<template>
    <v-layout class="rounded rounded-md">
  
      <v-navigation-drawer>
        <v-list>
          <v-list-item title=""></v-list-item>
        </v-list>
      </v-navigation-drawer>
  
      <v-navigation-drawer location="right">
        <v-list>
          <v-list-item title=""></v-list-item>
        </v-list>
      </v-navigation-drawer>
  
      <v-main class="d-flex align-center justify-center" style="min-height: 300px;">
        <NuxtPage />
        <!-- Main Content -->
      </v-main>
    </v-layout>
  </template>