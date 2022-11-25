<template>
  <v-app>
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
              Pasien
            </div>
            <v-divider></v-divider>
          </v-col>
        </v-row>
      </section>
      <div class="py-4">
        <v-card class="py-4">
          <v-card-title>
            <v-row>
              <v-col cols="6" md="7">
                <v-text-field
                  v-model="search"
                  filled
                  rounded
                  prepend-inner-icon="mdi-magnify"
                  label="Search"
                  single-line
                  hide-details
                ></v-text-field>
              </v-col>
              <v-col cols="3" md="2">
                <v-btn
                  color="primary"
                  elevation="4"
                  tile
                  outlined
                  raised
                  class="hidden-sm-and-down rounded-lg"
                  to="user/add"
                >
                  <v-icon left>mdi-plus</v-icon> Tambah
                </v-btn>
                <!-- Mobile Only -->
                <v-btn
                  color="primary"
                  elevation="4"
                  fab
                  outlined
                  raised
                  rounded
                  to="user/add"
                  class="hidden-sm-and-up"
                >
                  <v-icon>mdi-plus</v-icon>
                </v-btn>
              </v-col>
              <v-col cols="3" md="3">
                <v-btn
                  color="primary"
                  elevation="4"
                  tile
                  outlined
                  raised
                  class="hidden-sm-and-down rounded-lg"
                  to=""
                >
                  <v-icon left>mdi-file-excel-outline</v-icon> Export to Excel
                </v-btn>
                <!-- Mobile Only -->
                <v-btn
                  color="primary"
                  elevation="4"
                  fab
                  outlined
                  raised
                  rounded
                  to=""
                  class="hidden-sm-and-up"
                >
                  <v-icon>mdi-file-excel-outline</v-icon>
                </v-btn>
              </v-col>
            </v-row>
          </v-card-title>
          <v-container>
            <v-data-table
              :loading="loading"
              loading-text="Loading... Please wait"
              :headers="headers"
              :items="users"
              :search="search"
              hide-default-footer
            >
              <template v-slot:[`item.actions`]="{ item }">
                <v-icon color="primary" class="mr-4" @click="editData(item.id)"
                  >mdi-pencil</v-icon
                >
                <v-icon color="error" @click="deleteData(item.id)">mdi-delete</v-icon>
              </template>
            </v-data-table>
          </v-container>
          <div class="text-center pb-4">
            <v-pagination
              @click="getDataUser"
              @input="handlePageChange"
              v-model="page"
              :length="9"
              :total-visible="7"
              circle
            ></v-pagination>
          </div>
          <v-snackbar v-model="snackbar" :multi-line="multiLine">
            {{ text }}
            <template v-slot:action="{ attrs }">
              <v-btn color="red" text v-bind="attrs" @click="snackbar = false"> Close </v-btn>
            </template>
          </v-snackbar>
        </v-card>
      </div>
    </v-container>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      page: 1,
      options: {},
      multiLine: true,
      snackbar: false,
      text: `Berhasil Menghapus Data`,
      id: this.$route.params.id,
      search: "",
      users: [],
      loading: true,
      headers: [
        { text: "Title", align: "start", value: "title" },
        { text: "First Name", value: "firstName" },
        { text: "Last Name", value: "lastName" },
        { text: "Actions", value: "actions", sortable: false },
      ],
    };
  },
  methods: {
    getDataUser() {
      this.loading = true;
      let pageNumber = this.page - 1;
      axios({
        method: "get",
        url: "https://dummyapi.io/data/v1/user?limit=10&page=" + pageNumber,
        headers: {
          "app-id": "637ed92222ecff0b1c016f28",
        },
      })
        .then((response) => {
          console.log(response.data);
          this.users = response.data.data.map(this.getUserData);
          this.loading = false;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    refresh() {
      this.getDataUser();
    },

    editData(id) {
      this.$router.push({ name: "edituser", params: { id: id } });
    },

    deleteData(id) {
      axios
        .delete("https://dummyapi.io/data/v1/user/" + id, {
          headers: {
            "app-id": "637ed92222ecff0b1c016f28",
          },
        })
        .then((response) => {
          this.currentData = response.data;
          this.refresh();
          this.snackbar = true;
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

    handlePageChange(value) {
      this.currentPage = value;
      this.getDataUser();
    },
  },

  mounted() {
    this.getDataUser();
  },
};
</script>
