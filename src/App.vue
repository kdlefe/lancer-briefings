<template>
  <Header :header="this.header" />
  <div class="content-container">
    <section class="section-container" id="missions" style="width:435px; height:714px;">
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
            v-for="item in this.missions"
            :key="item.slug"
            :mission="item"
            :selected="this.mission_slug"
            @click="selectMission(item)"
          />
        </div>
      </div>
    </section>
    <section class="section-container" id="events" style="width:435px; height:714px;">
      <div class="section-header clipped-medium-backward">
        <img src="/icons/events-icon.svg" />
        <h1>Events Log</h1>
      </div>
      <div class="section-content-container">
        <Markdown :source="events" class="markdown" />
      </div>
    </section>
    <section class="section-container" id="pilots" style="width:894px; height:714px;">
      <div style="height:52px; overflow:hidden;">
        <div class="section-header clipped-medium-backward-pilot">
          <img src="/icons/pilot-icon.svg" />
          <h1>Pilot Roster</h1>
        </div>
        <div class="rhombus-back">&nbsp;</div>
      </div>
      <div class="section-content-container">
        <div class="pilot-list-container">
          <Pilot v-for="item in this.pilots" :key="item.slug" :pilot="item" />
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
  <Footer/>
</template>

<script>
import Header from './components/layout/Header.vue';
import Footer from './components/layout/Footer.vue';
import Mission from './components/Mission.vue';
import Pilot from './components/Pilot.vue';
import Markdown from 'vue3-markdown-it';

export default {
  components: {
    Header,
    Footer,
    Mission,
    Pilot,
    Markdown
  },

  data() {
    return {
      "mission_slug": "001",
      "current_md": "",
      "events": "",
      "missions": [
             {
          "slug": "001",
          "name": "Graveyard Shift",
          "status": "start"
        },
      ],
      "pilots": [
        {
          "callsign": "Rohe",
          "alias": "Paiti Arona",
          "code": "3mbf-sfn3k-76rprib1sw6jbf5lr-dnb5yw --- //      //INDEPENDENT MERCENARY",
          "corpro": "General Massive Systems",
          "frame": "Everest",
          "mech": "TBD"
        },
        {
          "callsign": "Verge",
          "alias": "Bishop Huxley",
          "code": "rvdt-nju64-paz1a9btqipesptwgw-7h6vm --- //      //INDEPENDENT MERCENARY",
          "corpro": "General Massive Systems",
          "frame": "Everest",
          "mech": "Radar Rider"
        },
                {
          "callsign": "Updog",
          "alias": "Nephili Rex-Montague",
          "code": "4zpd-jq8r7-19mxe2tw7c4hul9k-vns9qe --- //      //Karrakin House Montague",
          "corpro": "Smith-Shimano Corpro",
          "frame": "Aurelian",
          "mech": "Upgod"
        },
                {
          "callsign": "Scrub",
          "alias": "Walter Winkler",
          "code": "7ktb-fwl2d-83crv7jp1f9xdyza-qmr4bt --- //      //Colonial Capital",
          "corpro": "General Massive Systems",
          "frame": "Chomolungma",
          "mech": "Paperweight"
        },
        {
          "callsign": "Joe",
          "alias": "TBD",
          "code": "2nbx-hc5wq-05ytlmz4r1aqe7df-bpz3gm --- //      //TBD",
          "corpro": "General Massive Systems",
          "frame": "Everest",
          "mech": "TBD"
        },
     {
          "callsign": "TBD",
          "alias": "TBD",
          "code": "6jvm-szr9p-28keqdut8m0xrwln-hqc7du --- //      //Karrakin House Montague",
          "corpro": "General Massive Systems",
          "frame": "Everest",
          "mech": "TBD"
        },
      ],
      "header": {
        "planet": "Space",
        "year": "5017u",
        "system": "Dawnline Cluster",
        "gate": "Annamite-Rao Co",
        "ring": "Annamite-Line",
        "headerTitle": "Union Intelligence Bureau",
        "headerSubtitle": " ",
        "subheaderTitle": "Mission Dossier",
        "subheaderSubtitle": "MINDLOCK Contingent File",
      },
      "options":{
        "eventsMarkdownPerMission": true
      }
    }
  },

  created() {
    this.loadMissionMarkdown()
    this.loadEventsMarkdown()
  },

  computed: {

  },

  methods: {
    selectMission(mission) {
      this.mission_slug = mission.slug;
      this.loadMissionMarkdown()
      if(this.options.eventsMarkdownPerMission){
        this.loadEventsMarkdown();
      }
    },
    loadMissionMarkdown() {
      let self = this;
      let md = `/missions/${self.mission_slug}.md`
      var client = new XMLHttpRequest();
      client.open('GET', md);
      client.onreadystatechange = function () {
        self.current_md = client.responseText;
      }
      client.send();
    },
    loadEventsMarkdown() {
      let self = this;
      let md = "";

      if(self.options.eventsMarkdownPerMission){
        md = `/events/${self.mission_slug}.md`
      }
      else {
        md = "/events.md"
      }

      var client = new XMLHttpRequest();
      client.open('GET', md);
      client.onreadystatechange = function () {
        self.events = client.responseText;
      }
      client.send();
    }
  }

}
</script>


<style lang="scss">
#app {
  position: relative;
   width: 100vw; /* Use viewport width */
  height: 100vh; /* Use viewport height */
  overflow: auto;
  display: flex;
  flex-direction: column;
 }
  .header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 999; /* Adjust the z-index value as needed */
}
.container {
  display: flex;
   flex-wrap: wrap;
   flex-basis: 50%;
  justify-content: center;
}

.section {
  flex: 1 1 100%;
  box-sizing: border-box;
  padding: 20px;
}

  .col-mobile {
  width: 100%;
}

  @media (max-width: 767px) {
  /* Styles for mobile devices */

 #app {
    display: flex;
    flex-direction: column;
    align-items: center;
    min-height: 100vh;
    position: relative;
    height: 100vh;
    overflow-x: hidden;
  }

  .content-container {
    padding: 20px;
    box-sizing: border-box;
    margin: 0 auto;
    display: flex;
    max-width: 1000px; /* Set a maximum width for the content container */
    width: 100%; /* Set the initial width to 100% */
    padding: 3em;
    box-sizing: border-box;
    flex-direction: column;
    flex: 1 1 auto;
  }

  #missions,
  #events,
  #pilots {
    width: 100%;
    height: auto;
    margin-bottom: 20px;
  }

  .section {
    flex: 1 1 100%;
    box-sizing: border-box;
    padding: 20px;
  }

  .section-header {
    text-align: center;
    margin-bottom: 10px;
  }

  .section-header img {
    max-width: 50px;
    margin-right: 10px;
  }

 .section-container {
    width: 100%;
    margin-left: 0 !important;
    margin-right: 0 !important;
    padding-left: 16px;
    padding-right: 16px;
  }

  .section-content-container {
    padding: 10px;
  }

  .pilot-list-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
.section-container {
  #pilots {
    height: 34px;
    overflow: hidden;
  }
}
  .markdown {
    /* Styles for Markdown content */
  }

  .missions {
    flex-basis: 100%;
    order: 1;
  }

  .events {
    flex-basis: 100%;
    order: 2;
  }

  .pilots {
    flex-basis: 100%;
    order: 3;
  }
  #pilots .section-header.clipped-medium-backward {
    height: 86px;
    overflow-y: hidden;
  margin-bottom: -61px;
  }
  }
</style>
