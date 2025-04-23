<template>
  <div id="app">
    <Header :header="header" />
    <div class="content-container">
      <section class="section-container" id="missions">
        <div class="section-header clipped-medium-backward">
          <img src="/icons/mission-icon.svg" />
          <h1>Mission Log</h1>
        </div>
        <div class="section-content-container">
          <h3>Current Assignment</h3>
          <Markdown :source="current_md" class="markdown" />
          <h3>Mission List</h3>
          <div class="mission-list-container">
            <Mission
              v-for="item in missions"
              :key="item.slug"
              :mission="item"
              :selected="mission_slug"
              @click="selectMission(item)"
            />
          </div>
        </div>
      </section>
      <section class="section-container" id="events">
        <div class="section-header clipped-medium-backward">
          <img src="/icons/events-icon.svg" />
          <h1>Events Log</h1>
        </div>
        <div class="section-content-container">
          <Markdown :source="events" class="markdown" />
        </div>
      </section>
      <section class="section-container" id="pilots">
        <div class="pilot-header">
          <div class="section-header clipped-medium-backward-pilot">
            <img src="/icons/pilot-icon.svg" />
            <h1>Pilot Roster</h1>
          </div>
          <div class="rhombus-back">&nbsp;</div>
        </div>
        <div class="section-content-container">
          <div class="pilot-list-container">
            <Pilot v-for="item in pilots" :key="item.slug" :pilot="item" />
          </div>
        </div>
      </section>
    </div>
    <svg
      style="visibility: hidden; position: absolute;"
      width="0"
      height="0"
      xmlns="http://www.w3.org/2000/svg"
      version="1.1"
    >
      <defs>
        <filter id="round">
          <feGaussianBlur in="SourceGraphic" stdDeviation="5" result="blur" />
          <feColorMatrix
            in="blur"
            mode="matrix"
            values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 19 -5"
            result="goo"
          />
          <feComposite in="SourceGraphic" in2="goo" operator="atop" />
        </filter>
      </defs>
    </svg>
    <audio autoplay>
      <source src="/startup.ogg" type="audio/ogg" />
    </audio>
    <Footer />
  </div>
</template>

<script>
import Header from "./components/layout/Header.vue";
import Footer from "./components/layout/Footer.vue";
import Mission from "./components/Mission.vue";
import Pilot from "./components/Pilot.vue";
import Markdown from "vue3-markdown-it";

export default {
  components: {
    Header,
    Footer,
    Mission,
    Pilot,
    Markdown,
  },

  data() {
    return {
      mission_slug: "001",
      current_md: "",
      events: "",
      missions: [
        {
          slug: "001",
          name: "Graveyard Shift",
          status: "start",
        },
      ],
      pilots: [
        {
          callsign: "Rohe",
          alias: "Paiti Arona",
          code:
            "3mbf-sfn3k-76rprib1sw6jbf5lr-dnb5yw --- //      //INDEPENDENT MERCENARY",
          corpro: "General Massive Systems",
          frame: "Everest",
          mech: "TBD",
        },
        {
          callsign: "Verge",
          alias: "Bishop Huxley",
          code:
            "rvdt-nju64-paz1a9btqipesptwgw-7h6vm --- //      //INDEPENDENT MERCENARY",
          corpro: "General Massive Systems",
          frame: "Everest",
          mech: "Radar Rider",
        },
        {
          callsign: "Updog",
          alias: "Nephili Rex-Montague",
          code:
            "4zpd-jq8r7-19mxe2tw7c4hul9k-vns9qe --- //      //Karrakin House Montague",
          corpro: "Smith-Shimano Corpro",
          frame: "Aurelian",
          mech: "Upgod",
        },
        {
          callsign: "Scrub",
          alias: "Walter Winkler",
          code:
            "7ktb-fwl2d-83crv7jp1f9xdyza-qmr4bt --- //      //Colonial Capital",
          corpro: "General Massive Systems",
          frame: "Chomolungma",
          mech: "Paperweight",
        },
        {
          callsign: "Joe",
          alias: "TBD",
          code:
            "2nbx-hc5wq-05ytlmz4r1aqe7df-bpz3gm --- //      //TBD",
          corpro: "General Massive Systems",
          frame: "Everest",
          mech: "TBD",
        },
        {
          callsign: "TBD",
          alias: "TBD",
          code:
            "6jvm-szr9p-28keqdut8m0xrwln-hqc7du --- //      //Karrakin House Montague",
          corpro: "General Massive Systems",
          frame: "Everest",
          mech: "TBD",
        },
      ],
      header: {
        planet: "Space",
        year: "5017u",
        system: "Dawnline Cluster",
        gate: "Annamite-Rao Co",
        ring: "Annamite-Line",
        headerTitle: "Union Intelligence Bureau",
        headerSubtitle: " ",
        subheaderTitle: "Mission Dossier",
        subheaderSubtitle: "MINDLOCK Contingent File",
      },
      options: {
        eventsMarkdownPerMission: true,
      },
    };
  },

  created() {
    this.loadMissionMarkdown();
    this.loadEventsMarkdown();
  },

  methods: {
    selectMission(mission) {
      this.mission_slug = mission.slug;
      this.loadMissionMarkdown();
      if (this.options.eventsMarkdownPerMission) {
        this.loadEventsMarkdown();
      }
    },
    loadMissionMarkdown() {
      const md = `/missions/${this.mission_slug}.md`;
      const client = new XMLHttpRequest();
      client.open("GET", md);
      client.onreadystatechange = () => {
        if (client.readyState === 4 && client.status === 200) {
          this.current_md = client.responseText;
        }
      };
      client.send();
    },
    loadEventsMarkdown() {
      let md = "";
      if (this.options.eventsMarkdownPerMission) {
        md = `/events/${this.mission_slug}.md`;
      } else {
        md = "/events.md";
      }
      const client = new XMLHttpRequest();
      client.open("GET", md);
      client.onreadystatechange = () => {
        if (client.readyState === 4 && client.status === 200) {
          this.events = client.responseText;
        }
      };
      client.send();
    },
  },
};
</script>

<style lang="scss">
html,
body,
#app {
  height: 100%;
  margin: 0;
  padding: 0;
}

/* Set the root element as a full-screen flex container */
#app {
  display: flex;
  flex-direction: column;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

.header {
  flex: 0 0 auto;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 999;
}

/* The content container fills the available space beneath the header */
.content-container {
  display: flex;
  flex: 1 1 auto;
  flex-direction: row;
  overflow: hidden;
  margin-top: 60px; /* Adjust this value to your Header's height */
}

/* Each section is set to flex and its content scrolls if needed */
.section-container {
  flex: 1 1 0;
  display: flex;
  flex-direction: column;
  margin: 5px;
  overflow: hidden;
}
#missions {
  max-width: 500px;
  flex: 0 0 auto;
}
.section-content-container {
  flex: 1 1 auto;
  overflow: auto;
}

/* Pilot section specific adjustments */
.pilot-header {
  display: flex;
  flex-direction: column;
}

/* Mobile adjustments */
@media (max-width: 767px) {
  #app {
    flex-direction: column;
  }

  .content-container {
    flex-direction: column;
    margin-top: 0;
    padding: 20px;
  }

  .section-container {
    width: 100%;
    height: auto;
    margin-bottom: 20px;
  }
}

/* You can keep or adjust existing styles for headers, icons, markdown etc. */
</style>
