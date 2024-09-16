<template>
    <template>
      <div class="text-center pa-4">
  
      <v-dialog
        v-model="dialog"
        width="auto"
      >
        <v-card
          max-width="400"
          prepend-icon="mdi-update"
          text="บันทึกสำเร็จ"
          title="สถานะการบันทึก"
        >
            <template v-slot:actions>
              <v-btn
                class="ms-auto"
                text="Ok"
                @click="dialog = false"
              ></v-btn>
            </template>
          </v-card>
        </v-dialog>
      </div>
  
        <div class="text-center pa-4">
          <v-dialog
            v-model="dialogerror"
            width="auto"
          >
            <v-card
              max-width="400"
              prepend-icon="mdi-update"
              text="บันทึกไม่สำเร็จ"
              title="สถานะการบันทึก"
            >
                <template v-slot:actions>
                  <v-btn
                    class="ms-auto"
                    text="Ok"
                    @click="dialogerror = false"
                  ></v-btn>
                </template>
              </v-card>
            </v-dialog>
        </div>
    </template>
  
      <v-sheet class="mx-auto" width="300">
    
        <br>
        <v-form ref="form">
          <v-text-field
            v-model="username"
            label="UserName"
            required
          ></v-text-field>
          <v-text-field
            v-model="password"
            label="Password"
            required
          ></v-text-field><v-text-field
            v-model="email"
            label="Email"
            required
          ></v-text-field>
          <v-text-field
            v-model="picture"
            label="Picture"
            required
          ></v-text-field>
          
          <v-select
            v-model="status"
            :items="items"
            :rules="[v => !!v || 'Item is required']"
            label="status"
            required
          ></v-select>
    
         
          <div class="d-flex flex-column">
            <v-btn
              class="mt-4"
              color="success"
              block
              @click="save"
            >
              SAVE
            </v-btn>
    
            <v-btn
              class="mt-4"
              color="error"
              block
              @click="reset"
            >
              Reset Form
            </v-btn>
            <br>
    
          </div>
        </v-form>
        <a 
            class="text-blue text-decoration-none"
            href="login"
            rel="noopener noreferrer"
            target="_blank"
          >
            back to login
          </a>
      </v-sheet>
    </template>
    <script>
     import axios from 'axios'
     definePageMeta({
  layout: "custom",
});
    export default {
      data: () => ({
        status_save : '',
        dialog: false,
        dialogerror : false,
        std:'',
        username: '',
        password: '',
        email: '',
        picture: '',
        status: null,
        items: [
          'Yes',
          'No',      
        ],
      }),
  
      methods: {
          async save () {
          let students = {
          username : this.username,
          password : this.password,
          email : this.email,
          status : this.status,
          picture : this.picture,
          }
          console.log(students);
        const response = await axios.post('http://localhost:7000/insert', students);
        this.std = response.data;
        console.log("std =", this.std);
        if(this.std.status == 'ok') {
          this.dialog = true;
          console.log("สำเร็จ");
          this.status_save = 'บันทึกสำเร็จ';
        }else{
          this.dialogerror = true;
          console.log("ไม่สำเร็จ:");
          this.status_save = 'บันทึกไม่สำเร็จ';
        }
  
       },
        reset () {
          this.$refs.form.reset()
  
        },
      },
    }
  </script>
  