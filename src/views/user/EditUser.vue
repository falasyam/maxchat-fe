<template>
  <v-app>
    <v-container
      :class="{ 'px-4': $vuetify.breakpoint.smAndUp, '': $vuetify.breakpoint.smAndDown }"
    >
      <v-form ref="form" lazy-validation>
        <v-container>
          <v-row>
            <v-col cols="12" md="4">
              <v-text-field
                v-model="currentData.firstName"
                :rules="[
                  (v) => !!v || 'First Name is required',
                  (v) => v.length >= 2 || 'First Name must be more than 2 characters',
                ]"
                :counter="10"
                label="First name"
                required
                filled
                rounded
              ></v-text-field>
            </v-col>

            <v-col cols="12" md="4">
              <v-text-field
                v-model="currentData.lastName"
                :rules="[
                  (v) => !!v || 'Last Name is required',
                  (v) => v.length >= 2 || 'Last Name must be more than 2 characters',
                ]"
                :counter="10"
                label="Last name"
                required
                filled
                rounded
              ></v-text-field>
            </v-col>

            <v-col cols="12" md="4">
              <v-text-field
                v-model="currentData.email"
                :rules="[
                  (v) => !!v || 'E-mail is required',
                  (v) => /.+@.+/.test(v) || 'E-mail must be valid',
                ]"
                label="E-mail"
                required
                filled
                rounded
              ></v-text-field>
            </v-col>
          </v-row>
        </v-container>
      </v-form>
      <v-btn color="primary" class="mt-3" @click="showDialog">Update</v-btn>
      <div class="py-2"></div>
      <v-alert :value="alert" type="success">{{ messages }}</v-alert>
      <!-- Dialog Konfirmasi -->
      <v-dialog v-model="dialog" max-width="290">
        <v-card>
          <v-card-title class="text-h5"> Perhatian! </v-card-title>
          <v-card-text> And yakin sudah memasukkan data dengan benar? </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="green darken-1" text @click="dialog = false"> Tidak </v-btn>
            <v-btn color="green darken-1" text @click="updateData"> Ya </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <!-- Dialog Berhasil -->
      <v-dialog v-model="dialogsuccess" max-width="290">
        <v-card>
          <v-card-title class="text-h5"> Berhasil! </v-card-title>
          <v-card-text> Data berhasil diperbarui </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="green darken-1" text @click="redUser">Ok</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-container>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      dialog: false,
      dialogsuccess: false,
      id: this.$route.params.id,
      currentData: null,
      alert: false,
      submit: false,
      messages: "",
    };
  },

  methods: {
    showDialog() {
      this.dialog = true;
    },

    sucDialog() {
      this.dialogsuccess = true;
    },

    redUser() {
      this.$router.push({ path: "/user", redirect: { name: "user" } });
    },

    getData() {
      axios
        .get("https://dummyapi.io/data/v1/user/" + this.id, {
          headers: {
            "app-id": "637ed92222ecff0b1c016f28",
          },
        })
        .then((response) => {
          this.currentData = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    updateData() {
      axios
        .put("https://dummyapi.io/data/v1/user/" + this.id, this.currentData, {
          headers: {
            "app-id": "637ed92222ecff0b1c016f28",
          },
        })
        .then((response) => {
          console.log(response.data);
          this.alert = true;
          this.messages = "The data was updated successfully!";
          this.dialog = false;
          this.sucDialog();
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },

  mounted() {
    this.getData(this.$route.params.id);
  },
};
</script>
