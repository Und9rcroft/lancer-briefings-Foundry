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
          "slug": "010",
          "name": "Local Recon",
          "status": "start"
        },
        {
          "slug": "011",
          "name": "Vault Hunter",
          "status": "start"
        },        
      ],
      "pilots": [
       {
          "callsign": "Ronin",
          "alias": "Richard Oller",
          "code": "462370be-bd0f-41c2-b667-cc75f3a59a96///NDL-C-DEEP-STATION//377308ad-ba23-410b-ae37-68a1fb5f8db4",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "Let's Talk"
        },
        {
          "callsign": "Wolf",
          "alias": "Ledia Soliday",
          "code": "12345",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "Reply All"
        },        
        {
          "callsign": "Trooper",
          "alias": "clone 180",
          "code": "8bf210aa-ef12-43b2-a1c8-bb47f3057ab9///NDL-C-STORM-HORIZON//b27e8f34-5e67-4fc0-9423-a12fd65cc701",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "My Regards"
        },
        {
          "callsign": "Trooper",
          "alias": "clone 181",
          "code": "9d3c84ef-a562-4b3f-928d-ff56b0c882a1///NDL-C-IRON-TITAN//c76a93f8-0b5e-4bca-b541-6d83c092ddc3",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "My Regards"
        },
        {
          "callsign": "Trooper",
          "alias": "clone 182",
          "code": "5e98d7bc-38fa-4823-aaf6-ed01f94f8c34///NDL-C-SHADOW-LANCE//d91eb72b-2a84-4ea9-b73c-7d29d40f8034",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "My Regards"
        },
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
