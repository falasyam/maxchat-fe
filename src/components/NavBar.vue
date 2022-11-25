<template>
  <v-navigation-drawer v-model="drawer" :mini-variant.sync="mini" permanent fixed>
    <v-list-item class="px-2">
      <v-list-item-avatar>
        <v-btn icon @click.stop="mini = !mini">
          <v-avatar color="red" @click.stop="mini = !mini">
            <span class="white--text text-h5">M</span>
          </v-avatar>
        </v-btn>
      </v-list-item-avatar>
      <v-list-item-title>Maxchat</v-list-item-title>
      <v-btn icon @click.stop="mini = !mini">
        <v-icon>mdi-chevron-left</v-icon>
      </v-btn>
    </v-list-item>

    <v-divider></v-divider>

    <v-list nav dense link>
      <v-list-item-group v-model="selectedItem" color="primary">
        <v-list-item v-for="item in items" :key="item.title" :to="item.link" link>
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title class="font-weight-bold">{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list-item-group>
    </v-list>
  </v-navigation-drawer>
</template>

<script>
export default {
  data() {
    return {
      drawer: true,
      selectedItem: 0,
      items: [
        { title: "Dashboard", icon: "mdi-home-city", link: "/" },
        { title: "Users", icon: "mdi-account-group-outline", link: "/user" },
      ],
      mini: true,
    };
  },
};
</script>
