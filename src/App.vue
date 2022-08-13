<template>
  <v-app>
    <v-system-bar app height=" 65" color="#FFFFFF" >
      <v-row>
        <v-col cols="7">
          <v-img
            class="shrink mr-2 mt-10"
            contain
            src="https://stg.kross.app/images/svg/logo_kross.svg"
            transition="scale-transition"
            width="70"
          />
        </v-col>
        <v-col cols="3">
          <v-text-field
            label="Search project,content or client"
            class="mt-8 ml-16 rounded-lg"
            outlined
            dense
            small
            prepend-inner-icon="mdi-magnify"
          ></v-text-field>
        </v-col>
        <v-icon class="ml-5" size="20"> mdi-bell-badge-outline </v-icon>
        <v-col col="2">
          <v-list-item two-line class="mt-4 ml-2">
            <v-list-item-avatar>
              <img src="https://randomuser.me/api/portraits/women/81.jpg" />
            </v-list-item-avatar>

            <v-list-item-content>
              <v-list-item-title>Jane Smith</v-list-item-title>
              <v-list-item-subtitle>Account Maneger</v-list-item-subtitle>
            </v-list-item-content>
            <v-icon large> mdi-chevron-down </v-icon>
          </v-list-item>
        </v-col>
      </v-row>
    </v-system-bar>

    <v-divider></v-divider>

    <v-navigation-drawer v-model="drawer" app width="450">
      <ProjectComponentVue></ProjectComponentVue>
    </v-navigation-drawer>

    <v-main>
      <CalendarView></CalendarView>
    </v-main>
  </v-app>
</template>

<script>
import CalendarView from "@/components/CalendarView.vue";
import ProjectComponentVue from "@/components/ProjectComponent.vue";


export default {
  name: "App",

  components: {
    CalendarView,
    ProjectComponentVue,
   
},

  data: () => ({
    //
    data: () => ({ drawer: null }),
  }),
};
</script>
