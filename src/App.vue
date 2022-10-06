<template>
  <v-app>
    <v-navigation-drawer app class="pt-4" mini-variant expand-on-hover>
      <v-list>
        <v-list-item class="px-2">
          <v-list-item-avatar>
            <v-img src="https://github.com/isaelvvi.png"></v-img>
          </v-list-item-avatar>
        </v-list-item>

        <v-list-item link>
          <v-list-item-content>
            <v-list-item-title class="text-h6">
              PokeApi by Isael
            </v-list-item-title>
            <v-list-item-subtitle>isaelvvi@gmail.com</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <v-divider></v-divider>

      <v-list nav dense>
        <v-list-item to="/pokedex" link>
            <v-list-item-icon>
              <v-icon>mdi-list-box</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Pokedex</v-list-item-title>
          </v-list-item>
          <!-- <v-list-item link>
            <v-list-item-icon>
              <v-icon>mdi-account-multiple</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Shared with me</v-list-item-title>
          </v-list-item>
          <v-list-item link>
            <v-list-item-icon>
              <v-icon>mdi-star</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Starred</v-list-item-title>
          </v-list-item> -->
      </v-list>
    </v-navigation-drawer>
    <v-bottom-navigation class="d-flex d-sm-none" fixed>
      <v-btn style="background-color: transparent">
      <span>Team</span>

      <v-icon>mdi-history</v-icon>
      </v-btn>

      <v-btn to="/" style="background-color: transparent; bottom: 2rem; border-radius: 45px; height: 60px" class="button-float" absolute>        

        <v-img :src="pokeball" contain width="60" dark></v-img>
    
      </v-btn>

    <v-btn style="background-color: transparent">
      <span>Regions</span>

      <v-icon>mdi-map-marker</v-icon>
    </v-btn>
    </v-bottom-navigation>

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      pokeball: 'https://cdn-icons-png.flaticon.com/512/361/361998.png'
    };
  },
};
</script>

<style>

</style>