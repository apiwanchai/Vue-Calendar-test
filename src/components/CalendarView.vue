<template>
  <div>
    <v-toolbar class="">
      <p class="text-h6 mt-4">Content |</p>
      <v-btn-toggle
        v-model="toggle_multiple"
        color="primary"
        dense
        group
        multiple
      >
        <v-btn class="rounded-lg" large>
          <v-icon> mdi-calendar-text</v-icon> Calendar
        </v-btn>

        <v-btn :value="2" large class="rounded-lg">
          <v-icon>mdi-view-week-outline</v-icon> Board
        </v-btn>
      </v-btn-toggle>
      <v-spacer></v-spacer>

      <span class="primary--text mr-3"
        ><v-icon color="primary">mdi-archive-check-outline</v-icon> Submit for
        review</span
      >

      <v-btn color="primary" class="rounded-lg" @click="drawer = !drawer">
        <v-icon>mdi-plus</v-icon> Ceatre content
      </v-btn>
    </v-toolbar>
    <v-divider></v-divider>
    <v-row class="fill-height">
      <v-col>
        <v-sheet height="64" color="F5F9FD">
          <v-toolbar flat>
            <v-btn
              outlined
              class="mr-4"
              color="grey darken-2"
              @click="setToday"
            >
              Today
            </v-btn>
            <v-btn fab text small color="grey darken-2" @click="prev">
              <v-icon small> mdi-chevron-left </v-icon>
            </v-btn>
            <v-btn fab text small color="grey darken-2" @click="next">
              <v-icon small> mdi-chevron-right </v-icon>
            </v-btn>
            <v-toolbar-title v-if="$refs.calendar">
              {{ $refs.calendar.title }}
            </v-toolbar-title>
            <v-spacer></v-spacer>
            <v-menu bottom right>
              <template v-slot:activator="{ on, attrs }">
                <v-btn outlined color="grey darken-2" v-bind="attrs" v-on="on">
                  <span>{{ typeToLabel[type] }}</span>
                  <v-icon right> mdi-menu-down </v-icon>
                </v-btn>
              </template>
              <v-list>
                <v-list-item @click="type = 'day'">
                  <v-list-item-title>Day</v-list-item-title>
                </v-list-item>
                <v-list-item @click="type = 'week'">
                  <v-list-item-title>Week</v-list-item-title>
                </v-list-item>
                <v-list-item @click="type = 'month'">
                  <v-list-item-title>Month</v-list-item-title>
                </v-list-item>
                <v-list-item @click="type = '4day'">
                  <v-list-item-title>4 days</v-list-item-title>
                </v-list-item>
              </v-list>
            </v-menu>
          </v-toolbar>
        </v-sheet>
        <v-sheet height="600">
          <v-calendar
            ref="calendar"
            v-model="focus"
            color="primary"
            :events="events"
            :event-color="getEventColor"
            :type="type"
            :now="today"
            @click:event="showEvent"
            @click:more="viewDay"
            @click:date="drawer = !drawer"
            @change="updateRange"
          ></v-calendar>

          <v-navigation-drawer
            v-model="drawer"
            absolute
            right
            temporary
            width="650"
            
          >
            <FormCalendarVue v-if="drawer != false"></FormCalendarVue>
          </v-navigation-drawer>
        </v-sheet>
      </v-col>
    </v-row>
  </div>
</template>

<script>
// import { fa } from 'vuetify/lib/locale'
import axios from "axios";
import FormCalendarVue from "./FormCalendar.vue";
export default {
  data: () => ({
    focus: new Date().toISOString().substr(0, 10),
    type: "month",
    drawer: false,
    today: new Date().toISOString().substr(0, 10),
    typeToLabel: {
      month: "Month",
      week: "Week",
      day: "Day",
      "4day": "4 Days",
    },
    selectedEvent: {},
    selectedElement: null,
    selectedOpen: false,
    name: null,
    detail: null,
    start: null,
    end: null,
    currentlyEditting: null,
    events: [],
    dailog: false,
    color: "black",
    names: [],
  }),
  mounted() {
    // this.$refs.calendar.checkChange()
    this.getEvents();
  },
  watch: {
    group() {
      this.drawer = false;
    },
  },
  methods: {
    getEvents() {
      let events = [];
      axios.get(" http://localhost:3002/calendarusers").then((snap) => {
        snap.data.map((doc) => {
          let appData = doc;
          events.push(appData);
        });
      });
      this.events = events;
      console.log(this.events);
    },

    viewDay({ date }) {
      this.focus = date;
      this.type = "day";
    },
    getEventColor(event) {
      return event.color;
    },
    setToday() {
      this.focus = "";
    },
    prev() {
      this.$refs.calendar.prev();
    },
    next() {
      this.$refs.calendar.next();
    },
    showEvent({ nativeEvent, event }) {
      const open = () => {
        this.selectedEvent = event;
        this.selectedElement = nativeEvent.target;
        requestAnimationFrame(() =>
          requestAnimationFrame(() => (this.selectedOpen = true))
        );
      };

      if (this.selectedOpen) {
        this.selectedOpen = false;
        requestAnimationFrame(() => requestAnimationFrame(() => open()));
      } else {
        open();
      }

      nativeEvent.stopPropagation();
    },
    updateRange({ start, end }) {
      const events = [];

      const min = new Date(`${start.date}T00:00:00`);
      const max = new Date(`${end.date}T23:59:59`);
      const days = (max.getTime() - min.getTime()) / 86400000;
      const eventCount = this.rnd(days, days + 20);

      for (let i = 0; i < eventCount; i++) {
        const allDay = this.rnd(0, 3) === 0;
        const firstTimestamp = this.rnd(min.getTime(), max.getTime());
        const first = new Date(firstTimestamp - (firstTimestamp % 900000));
        const secondTimestamp = this.rnd(2, allDay ? 288 : 8) * 900000;
        const second = new Date(first.getTime() + secondTimestamp);

        events.push({
          name: this.names[this.rnd(0, this.names.length - 1)],
          start: first,
          end: second,
          color: this.colors[this.rnd(0, this.colors.length - 1)],
          timed: !allDay,
        });
      }

      this.events = events;
    },
    rnd(a, b) {
      return Math.floor((b - a + 1) * Math.random()) + a;
    },
  },
  components: {
    FormCalendarVue: FormCalendarVue,
  },
};
</script>