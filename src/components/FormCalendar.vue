<template>
  <div class="pa-10">
    <v-row>
      <v-col col="7">
        <p class="text-h6">
          <v-icon color="black">mdi-view-grid-outline</v-icon> Create Content
        </p>
      </v-col>
      <v-col col="4" class="d-flex justify-end">
        <v-btn class="text-subtitle-2 rounded-lg" color="primary" large>
          <v-icon class="mr-2"> mdi-checkbox-marked-outline </v-icon> Create
          Content
        </v-btn>
      </v-col>
    </v-row>
    <v-form ref="form" v-model="valid" lazy-validation class="pa-5">
      <v-select
        v-model="selectProject"
        :rules="nameRules"
        label="Select project"
        filled
        :items="dataProject"
        item-text="projectName"
        :menu-props="{ bottom: true, offsetY: true }"
        color="grey"
        class="font-weight-medium"
        required
      ></v-select>

      <v-text-field
        v-model="topicName"
        :rules="emailRules"
        filled
        color="grey"
        class="font-weight-medium"
        label="Topic name"
        required
      ></v-text-field>

      <v-row>
        <v-col col="6">
          <v-menu
            v-model="menu"
            :close-on-content-click="false"
            :nudge-right="40"
            transition="scale-transition"
            offset-y
            min-width="auto"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="dateStart"
                label="Due date & time"
                append-icon="mdi-calendar"
                readonly
                v-bind="attrs"
                color="grey"
                class="font-weight-medium"
                v-on="on"
                filled
              ></v-text-field>
            </template>
            <v-date-picker
              v-model="dateStart"
              @input="menu = false"
              :min="new Date().toISOString().substr(0, 10)"
            ></v-date-picker>
          </v-menu>
        </v-col>
        <v-col col="6">
          <v-menu
            v-model="menu2"
            :close-on-content-click="false"
            :nudge-right="40"
            transition="scale-transition"
            offset-y
            min-width="auto"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="dateEnd"
                label="Publishing date & time"
                append-icon="mdi-calendar"
                color="grey"
                class="font-weight-medium"
                readonly
                v-bind="attrs"
                filled
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker
              v-model="dateEnd"
              @input="menu2 = false"
              :min="new Date().toISOString().substr(0, 10)"
            ></v-date-picker>
          </v-menu>
        </v-col>
      </v-row>
      <v-text-field
        v-model="keyMessage"
        :rules="emailRules"
        label="Key Message"
        filled
        color="grey"
        class="font-weight-medium"
        required
      ></v-text-field>
      <v-text-field
        v-model="descripttion"
        :rules="emailRules"
        label="Descripttion"
        filled
        class="font-weight-medium"
        color="grey"
        required
      ></v-text-field>
     

      <p class="mt-5 text--darken-1 grey--text">Content format</p>

      <v-btn-toggle
        v-model="toggle_multiple"
        color="primary"
        dense
        group
        multiple
      >
        <v-btn class="rounded-lg" large>
          <v-icon> mdi-file-image-outline</v-icon> Image
        </v-btn>

        <v-btn :value="2" large class="rounded-lg">
          <v-icon>mdi-play-box-outline</v-icon> Vedio
        </v-btn>

        <v-btn :value="3" large class="rounded-lg">
          <v-icon>mdi-note-text-outline</v-icon> Writing
        </v-btn>
      </v-btn-toggle>
      <p class="mt-5 text--darken-1 grey--text">Post type</p>
      <v-btn-toggle
        v-model="toggle_multiple"
        color="primary"
        dense
        group
        multiple
      >
        <v-btn class="rounded-lg" large>
          <v-icon> mdi-arrow-up</v-icon> Boost post
        </v-btn>

        <v-btn :value="2" large class="rounded-lg">
          <v-icon>mdi-text</v-icon> Non-boost post
        </v-btn>
      </v-btn-toggle>
    </v-form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "VueclanedarFormCalendar",

  data() {
    return {
      dateStart: undefined,
      dateEnd: undefined,
      menu: false,
      menu2: false,
      isToggled: false,
     dataProject:undefined
    };
  },

  mounted() {
    this.getDataProject() 
  },

  methods: {
     getDataProject() {
      axios.get(" http://localhost:3002/project").then((snap) => {
        this.dataProject = snap.data;
        console.log(this.dataProject);
      });
    },
  },
};
</script>

<style scoped>

</style>