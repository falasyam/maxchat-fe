<template>
  <v-container
    :class="{ 'px-4': $vuetify.breakpoint.smAndUp, '': $vuetify.breakpoint.smAndDown }"
  >
    <section class="py-2">
      <v-row>
        <v-col cols="12">
          <div
            :class="{
              'display-1': $vuetify.breakpoint.smAndUp,
              title: $vuetify.breakpoint.smAndDown,
            }"
          >
            Dashboard
          </div>
          <v-divider></v-divider>
        </v-col>
      </v-row>
    </section>
    <section>
      <v-row class="py-12">
        <v-col cols="6" md="3">
          <v-card color="light-blue lighten-4" width="200px" class="py-4 elevation-0">
            <div class="text-center">
              <div class="title font-weight-bold light-blue--text text--darken-4">21</div>
              <div class="title font-weight-regular light-blue--text text--darken-4">
                Antre
              </div>
            </div>
          </v-card>
        </v-col>
        <v-col cols="6" md="3">
          <v-card color="yellow lighten-4" width="200px" class="py-4 elevation-0">
            <div class="text-center">
              <div class="title font-weight-bold yellow--text text--darken-4">{{ total }}</div>
              <div class="title font-weight-regular yellow--text text--darken-4">
                Terlayani
              </div>
            </div>
          </v-card>
        </v-col>
        <v-col cols="6" md="3">
          <v-card color="green lighten-4" width="200px" class="py-4 elevation-0">
            <div class="text-center">
              <div class="title font-weight-bold green--text text--darken-4">205 Pasien</div>
              <div class="title font-weight-regular green--text text--darken-4">Bulan Ini</div>
            </div>
          </v-card>
        </v-col>
        <v-col cols="6" md="3">
          <v-card color="lime lighteen-4" width="200px" class="py-4 elevation-0">
            <div class="text-center">
              <div class="title font-weight-bold lime--text text--darken-4">512 Pasien</div>
              <div class="title font-weight-regular lime--text text--darken-4">Bulan Lalu</div>
            </div>
          </v-card>
        </v-col>
      </v-row>
    </section>
    <section>
      <v-row>
        <v-col cols="12">
          <div
            :class="{
              'font-weight-bold display-1': $vuetify.breakpoint.smAndUp,
              'font-weight-bold title': $vuetify.breakpoint.smAndDown,
            }"
          >
            Pasien Selanjutnya
          </div>
        </v-col>
      </v-row>
      <div class="py-3"></div>
      <v-row>
        <v-col cols="12" md="6">
          <v-card class="px-4 py-4 rounded-lg">
            <v-data-table
              :loading="loading"
              loading-text="Loading... Please wait"
              :headers="headers"
              :items="users"
              disable-sort
              mobile-breakpoint="600"
              hide-default-footer
            >
            </v-data-table>
          </v-card>
        </v-col>
        <v-col cols="12" md="6">
          <v-card class="mx-auto text-center" color="blue" dark max-width="600">
            <v-card-text>
              <v-sheet color="rgba(0, 0, 0, .12)">
                <v-sparkline
                  :labels="labels"
                  :value="value"
                  color="rgba(255, 255, 255, .7)"
                  height="100"
                  padding="24"
                  stroke-linecap="round"
                  smooth
                >
                </v-sparkline>
              </v-sheet>
            </v-card-text>

            <v-card-text>
              <div class="text-h4 font-weight-thin">Statistik 7 Bulan</div>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </section>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "HomeView",

  components: {},
  data() {
    return {
      loading: false,
      users: [],
      total: "",
      headers: [
        { text: "Title", align: "start", value: "title" },
        { text: "First Name", value: "firstName" },
        { text: "Last Name", value: "lastName" },
      ],
      labels: ["Jan", "Feb", "Mar", "Mei", "Apr", "Jun", "Jul"],
      value: [423, 446, 675, 510, 590, 910, 760],
    };
  },

  methods: {
    getDataUser() {
      this.loading = true;
      axios({
        method: "get",
        url: "https://dummyapi.io/data/v1/user?limit=5",
        headers: {
          "app-id": "637ed92222ecff0b1c016f28",
        },
      })
        .then((response) => {
          console.log(response.data);
          this.users = response.data.data.map(this.getUserData);
          this.total = response.data.total;
          this.loading = false;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    getUserData(user) {
      return {
        id: user.id,
        title: user.title,
        firstName: user.firstName,
        lastName: user.lastName,
      };
    },
  },

  mounted() {
    this.getDataUser();
  },
};
</script>
