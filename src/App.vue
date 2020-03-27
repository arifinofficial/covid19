<template>
  <v-app>
    
    <v-app-bar app color="primary" dark>
      <!-- <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon> -->
    <v-btn to="/" color="blue-grey darken-3">
      <v-toolbar-title><strong>Covid-19</strong> Data</v-toolbar-title>
    </v-btn>

      <v-spacer></v-spacer>

      <v-btn icon to="/tentang">
        <v-icon>mdi-information-outline</v-icon>
      </v-btn>
    </v-app-bar>

    <!-- <v-card>
      <v-navigation-drawer app v-model="drawer">
        <v-list>
          <v-list-item v-for="(item, index) in menus" :key="index" :to="item.route">
            <v-list-item-icon>
              <v-icon left>{{ item.icon }}</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title>{{ item.title }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-navigation-drawer>
    </v-card> -->

    <v-content>
        <router-view></router-view>
    </v-content>

    <v-card>
      <v-footer absolute app>
        <v-card-text class="text-center">
          &copy; <strong>Arifin N</strong> {{ new Date().getFullYear() }} — Made with <v-icon color="red" small>mdi-heart</v-icon> in Denpasar, ID.
        </v-card-text>
      </v-footer>
    </v-card>

  </v-app>
</template>

<script>
export default {
  name: 'App',
  data: () => ({
    // drawer: false,
    menus: [
      { title: 'Home', icon: 'mdi-home', route: '/' },
      { title: 'Home', icon: 'mdi-account', route: '/about' }
    ],
  })
}
</script>