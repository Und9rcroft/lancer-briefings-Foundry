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
      "mission_slug": "000",
      "current_md": "000",
      "events": "000",
      "missions": [
        {
          "slug": "000",
          "name": "Training Wheels",
          "status": "success"
        },
        {
          "slug": "001",
          "name": "Bug Hunt",
          "status": "success"
        },
        {
          "slug": "003",
          "name": "Local Recon",
          "status": "success"
        },
        {
          "slug": "004",
          "name": "Vault Hunter",
          "status": "start"
        },        
      ],
      "pilots": [
       {
          "callsign": "Ronin",
          "alias": "Richard Oller",
          "code": "a9d452bf-ce18-4b62-bc4f-ef93d2087ba1///RNN-B-RONIN-BLADE//e5f7c320-8d14-4eac-b751-c92fd47dd802",
          "corpro": "IPS-N",
          "frame": "BLACKBEARD",
          "mech": "Ever Closer"
        },
        {
          "callsign": "Wolf",
          "alias": "Ledia Soliday",
          "code": "7a3dc50f-3268-4906-8d3f-7e4006b3cb83//NDL-C-UNBENT-SKY",
          "corpro": "HORUS",
          "frame": "PEGASUS",
          "mech": "Justice Shared Freely"
        },
        {
          "callsign": "Lobster",
          "alias": "Henry Saitomo",
          "code": "SEAFOOD-420-CLAWZL-LOB-BATTLE-CHEF//BUTTERED-UP-9000",
          "corpro": "SSC",
          "frame": "Monarch",
          "mech": "ロブスター (Robusutā)"
        },

        {
          "callsign": "Vega",
          "alias": "Isaac",
          "code": "5ca7e5b2-050d-451d-bfdb-4d5e9572e798//NDL-C-BARYON-GIFT",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "Fight Like You Mean It"
        },
        {
          "callsign": "Earlybird",
          "alias": "Hugh",
          "code": "f8da0964-42e7-4684-81f0-fe97aa87ef35//NDL-C-DEEP-CAGE",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "A Thousand Lives"
        },
        {
          "callsign": "Rosebud",
          "alias": "Tom",
          "code": "0f6c3342-016b-4700-b6e3-10fc8e1a5f0a//NDL-C-BLIND-NOVEMBER",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "Sled"
        },
        {
          "callsign": "Trooper",
          "alias": "clone 180",
          "code": "d3e712ac-bf47-49f8-9263-af84f4026ea2///TRP-A-TROOPER-FRONTLINE//f9a3d216-6c84-45ba-983a-d31fc25aa604",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "My Regards"
        },
        /*{
          "callsign": "",
          "alias": "",
          "code": "",
          "corpro": "",
          "frame": "",
          "mech": ""
        },
        {
          "callsign": "Trooper",
          "alias": "clone 182",
          "code": "5e98d7bc-38fa-4823-aaf6-ed01f94f8c34///NDL-C-SHADOW-LANCE//d91eb72b-2a84-4ea9-b73c-7d29d40f8034",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "My Regards"
        }, */
      ],
      "header": {
        "planet": "Hercynia",
        "year": "5016u",
        "system": "Ardennes-3",
        "gate": "Atlas-Quanokrim",
        "ring": "Atlas-Line",
        "headerTitle": "Goodman Ind",
        "headerSubtitle": "Research Company",
        "subheaderTitle": "Crisis Response",
        "subheaderSubtitle": "Delta-Echo-Echo-Zulu",
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
