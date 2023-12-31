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
          "name": "Bug-Hunt",
          "status": "start"
        },
      ],
      "pilots": [
        {
          "callsign": "Boots",
          "alias": "Magalie Namauu",
          "code": "qlzp-o36zs-jxks13cq1jvkkupbwc-43xmp // HARRISON ARMORY FIELD TEST PILOT",
          "corpro": "HORUS",
          "frame": "Gorgon",
          "mech": "Hush, Puppy"
        },
        {
          "callsign": "Henko",
          "alias": "Ziau Jua",
          "code": "aztf-1phqs-4t3zm333yjp44eh635-jf6az // UNION NAVY AUXILIARY CORPORAL",
          "corpro": "Harrison Armory",
          "frame": "Sherman",
          "mech": "Cost of Doing Business"
        },
        {
          "callsign": "Sigmundr",
          "alias": "Skorri Eydisson",
          "code": "3mbf-sfn3k-76rprib1sw6jbf5lr-dnb5yw // INDEPENDENT MERCENARY",
          "corpro": "Harrison Armory",
          "frame": "Tokugawa",
          "mech": "Baldr"
        },
        {
          "callsign": "Verge",
          "alias": "Bishop Huxley",
          "code": "rvdt-nju64-paz1a9btqipesptwgw-7h6vm // INDEPENDENT MERCENARY",
          "corpro": "Smith-Shimano Corpro",
          "frame": "Metalmark",
          "mech": "Danse Macabre"
        },
        {
          "callsign": "Skeptic",
          "alias": "Toska Adachi",
          "code": "1sqb-9whc9-1b2ibxtqeco97fbhl7-e5ocm // UNION NAVY AUXILIARY SERGEANT",
          "corpro": "Horus",
          "frame": "Pegasus",
          "mech": "Chimera"
        },
         {
          "callsign": "Pepper",
          "alias": "Kinera Warheit",
          "code": "ldgg-6epo4-n3lhl6vo0ex26qec5v-3dxaa // UNION NAVY AUXILIARY SERGEANT",
          "corpro": "GMS",
          "frame": "Chomolungma",
          "mech": "Kira"
        },
      ],
      "header": {
        "planet": "Hercynia",
        "year": "5014u",
        "system": "Ardennes-3",
        "gate": "Atlas-Quanokrim",
        "ring": "Atlas-Line",
        "headerTitle": "Union Naval Department",
        "headerSubtitle": " ",
        "subheaderTitle": "Mission Dossier",
        "subheaderSubtitle": "Hercynia Conflict",
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
  width: 1902px;
  height: 910px;
  overflow: hidden;
}
</style>
