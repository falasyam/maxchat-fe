<template>
  <v-app>
    <v-container
      :class="{ 'px-4': $vuetify.breakpoint.smAndUp, '': $vuetify.breakpoint.smAndDown }"
    >
      <v-form ref="form" lazy-validation>
        <v-container>
          <v-row>
            <v-col cols="12">
              <v-text-field
                v-model="adduser.firstName"
                :rules="[
                  (v) => !!v || 'First Name is required',
                  (v) => v.length >= 2 || 'First Name must be more than 2 characters',
                ]"
                label="First name"
                required
                filled
                rounded
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field
                v-model="adduser.lastName"
                :rules="[
                  (v) => !!v || 'Last Name is required',
                  (v) => v.length >= 2 || 'Last Name must be more than 2 characters',
                ]"
                label="Last name"
                required
                filled
                rounded
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field
                v-model="adduser.email"
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
      <v-btn color="primary" class="mt-3" @click="showDialog">Submit</v-btn>
      <!-- Dialog -->
      <v-dialog v-model="dialog" max-width="290">
        <v-card>
          <v-card-title class="text-h5"> Perhatian! </v-card-title>
          <v-card-text> And yakin sudah memasukkan data dengan benar? </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="green darken-1" text @click="dialog = false"> Tidak </v-btn>
            <v-btn color="green darken-1" text @click="save"> Ya </v-btn>
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
      adduser: {
        firstName: "",
        lastName: "",
        email: "",
      },
      submit: false,
    };
  },

  methods: {
    showDialog() {
      this.dialog = true;
    },

    save() {
      var data = {
        firstName: this.adduser.firstName,
        lastName: this.adduser.lastName,
        email: this.adduser.email,
      };

      axios
        .post("https://dummyapi.io/data/v1/user/create", data, {
          headers: {
            "app-id": "637ed92222ecff0b1c016f28",
          },
        })
        .then((response) => {
          this.adduser.firstName = response.data.firstName;
          this.adduser.lastName = response.data.lastName;
          this.adduser.email = response.data.email;
          console.log(response.data);
          this.submit = true;
          this.dialog = false;
          this.$router.push({ path: "/user", redirect: { name: "user" } });
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>
