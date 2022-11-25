<template>
  <v-app>
    <v-main>
      <!-- Navbar md and up -->
      <NavBar />
      <!-- Navbar mobile -->
      <section class="px-4 py-4 hidden-md-and-up">
        <v-app-bar class="rounded-lg" color="blue lighten-2" flat>
          <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>

          <v-toolbar-title></v-toolbar-title>

          <v-spacer></v-spacer>
          <v-menu offset-y rounded-lg transition="scale-transition" origin="right top">
            <template v-slot:activator="{ on, attrs }">
              <v-avatar color="red" size="36" v-bind="attrs" v-on="on">
                <span class="white--text">M</span>
              </v-avatar>
            </template>

            <v-list>
              <v-list-item v-for="(menu, index) in menus" :key="index">
                <v-list-item-icon>
                  <v-icon>{{ menu.icon }}</v-icon>
                </v-list-item-icon>
                <v-list-item-title>{{ menu.title }}</v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>
        </v-app-bar>
        <!-- Nav Drawer -->
        <v-navigation-drawer v-model="drawer" absolute temporary>
          <v-list-item class="py-4">
            <v-list-item-avatar>
              <v-avatar color="red" size="36">
                <span class="white--text">M</span>
              </v-avatar>
            </v-list-item-avatar>
            <v-list-item-title>Maxchat</v-list-item-title>
          </v-list-item>

          <v-list shaped link>
            <v-subheader>Menu</v-subheader>
            <v-list-item-group v-model="selectedItem" color="primary">
              <v-list-item v-for="item in items" :key="item.title" :to="item.link" link>
                <v-list-item-icon>
                  <v-icon>{{ item.icon }}</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  <v-list-item-title class="font-weight-bold">{{
                    item.title
                  }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list-item-group>
          </v-list>
        </v-navigation-drawer>
      </section>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
import NavBar from "../src/components/NavBar.vue";

export default {
  name: "App",
  components: {
    NavBar,
  },

  data: () => ({
    drawer: false,
    selectedItem: 0,
    items: [
      { title: "Dashboard", icon: "mdi-home-city", link: "/" },
      { title: "Pasien", icon: "mdi-account-group-outline", link: "/user" },
    ],
    menus: [
      { title: "Profil", icon: "mdi-account-outline" },
      { title: "Pengaturan", icon: "mdi-cog-outline" },
      { title: "Keluar", icon: "mdi-logout" },
    ],
  }),
};
</script>
