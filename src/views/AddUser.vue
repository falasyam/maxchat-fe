<template>
  <v-app>
    <v-container
      :class="{ 'px-4': $vuetify.breakpoint.smAndUp, 'px-16': $vuetify.breakpoint.smAndDown }"
    >
      <v-form ref="form" lazy-validation>
        <v-container>
          <v-row>
            <v-col cols="12" md="4">
              <v-text-field
                v-model="adduser.firstName"
                :rules="[(v) => !!v || 'Required']"
                :counter="10"
                label="First name"
                required
              ></v-text-field>
            </v-col>

            <v-col cols="12" md="4">
              <v-text-field
                v-model="adduser.lastName"
                :rules="[(v) => !!v || 'Required']"
                :counter="10"
                label="Last name"
                required
              ></v-text-field>
            </v-col>

            <v-col cols="12" md="4">
              <v-text-field
                v-model="adduser.email"
                :rules="[(v) => !!v || 'Required']"
                label="E-mail"
                required
              ></v-text-field>
            </v-col>
          </v-row>
        </v-container>
      </v-form>
      <v-btn color="primary" class="mt-3" @click="save">Submit</v-btn>
    </v-container>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      adduser: {
        firstName: "",
        lastName: "",
        email: "",
      },
      submit: false,
    };
  },

  methods: {
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
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>
