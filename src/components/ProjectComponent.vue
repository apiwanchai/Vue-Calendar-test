<template>
  <div>
    <v-navigation-drawer absolute color="grey lighten-3" mini-variant>
      <v-icon
        class="d-block text-center mx-auto mt-4"
        color="grey darken-1"
        outlited
        size="36"
        >mdi-bag-checked</v-icon
      >

      <v-divider class="mx-3 my-5"></v-divider>

      <v-icon
        class="d-block text-center mx-auto mt-4"
        color="grey darken-1"
        outlited
        size="28"
        >mdi-book</v-icon
      >
      <v-icon
        class="d-block text-center mx-auto mt-4"
        color="grey darken-1"
        outlited
        size="28"
        >mdi-gift-outline</v-icon
      >
      <v-icon
        class="d-block text-center mx-auto mt-4"
        color="grey darken-1"
        outlited
        size="28"
        >mdi-share-variant-outline</v-icon
      >
      <v-divider class="mx-3 my-5"></v-divider>
      <v-icon
        class="d-block text-center mx-auto mt-4"
        color="grey darken-1"
        outlited
        size="28"
        >mdi-equalizer-outline</v-icon
      >
      <v-icon
        class="d-block text-center mx-auto mt-4"
        color="grey darken-1"
        outlited
        size="28"
        >mdi-view-list-outline</v-icon
      >
      <v-divider class="mx-3 my-5"></v-divider>
      <v-icon
        class="d-block text-center mx-auto mt-4"
        color="grey darken-1"
        outlited
        size="28"
        >mdi-account</v-icon
      >

      <div style="position: absolute; bottom: 20px">
        <v-icon class="ml-3">mdi-cog-outline</v-icon>
      </div>
    </v-navigation-drawer>

    <v-divider></v-divider>
    <v-row>
      <v-col cols="6">
        <p class="d-block mx-auto mt-4 font-weight-bold">
          <v-icon class="ml-16 mr-4" color="grey darken-1" size="36"
            >mdi-bag-checked</v-icon
          >Project
        </p>
      </v-col>
      <v-col cols="3">
        <v-btn color="primary" class="rounded-lg ml-3 mt-4">
          <v-icon>mdi-plus</v-icon> Ceatre Project
        </v-btn>
      </v-col>
    </v-row>
    <v-divider></v-divider>
    <v-row>
      <v-col cols="8">
        <v-tabs class="ml-13">
          <v-tab>Active</v-tab>
          <v-tab>Inactive</v-tab>
        </v-tabs>
      </v-col>
      <v-col cols="4">
        <div class="ml-5">
          <v-btn-toggle color="primary" dense class="rounded-lg mt-2">
            <v-btn dense>
              <v-icon>mdi-filter-outline</v-icon>
            </v-btn>

            <v-btn dense>
              <v-icon>mdi-sort-variant</v-icon>
            </v-btn>
          </v-btn-toggle>
        </div>
      </v-col>
    </v-row>
    <v-divider></v-divider>

    <v-card
      class="mb-0 ml-12 post reply"
      outlined
      max-height="120"
      hover
      v-for="item in this.dataProject"
      :key="item"
    >
      <v-list-item three-line>
        <v-list-item-content>
          <v-list-item-title class="text-h6 mb-1">
            <v-row>
              <v-col cols="10">
                <v-divider vertical></v-divider> {{ item.projectName }}
              </v-col>
              <v-col cols="2">
                <v-icon>mdi-content-duplicate</v-icon>
              </v-col>
            </v-row>
          </v-list-item-title>

          <v-list-item-subtitle class="mt-1"
            >{{ item.detail }}
          </v-list-item-subtitle>
          <v-list-item-subtitle class="mt-3">
            <div class="d-flex">
              <p><v-icon>mdi-view-grid-outline</v-icon> 2/5</p>
              <v-divider vertical class="mb-5 ml-3"></v-divider>

              <v-icon class="ml-3 mb-5">mdi-facebook</v-icon>
              <v-icon class="ml-1 mb-5">mdi-instagram</v-icon>
              <v-icon class="ml-1 mb-5">mdi-twitter</v-icon>
              <v-icon class="ml-1 mb-5">mdi-share-variant-outline</v-icon>
              <v-spacer></v-spacer>
              <v-btn x-small outlined class="mr-2" color="success">KR</v-btn>
              <v-btn x-small outlined>3+</v-btn>
            </div>
          </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
    </v-card>
    <!-- <v-list class="pl-14" shaped>
      <v-list-item v-for="n in 5" :key="n" link>
        <v-list-item-content>
          <v-list-item-title>Item {{ n }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-list> -->
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "VueclanedarProjectComponent",

  data() {
    return {
      selectedItem: 1,
      dataProject: undefined,
      value: 1,
      isHovering: false,
      left: 0,
      items: [
        { text: "Real-Time", icon: "mdi-clock" },
        { text: "Audience", icon: "mdi-account" },
        { text: "Conversions", icon: "mdi-flag" },
      ],
    };
  },

  mounted() {
    this.getDataProject();
  },

  methods: {
    handleMouseEnter(e) {
      return (this.left = e.target.getBoundingClientRect().x - 8);
    },
    getDataProject() {
      axios.get(" http://localhost:3002/project").then((snap) => {
        this.dataProject = snap.data;
        console.log(this.dataProject);
      });
    },
  },
};
</script>

<style  scoped>
.post.reply:hover {
  border-left: 15px solid #1976d2 !important;
}
</style>