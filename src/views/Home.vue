<template>
  <v-app>
    <v-row>
      <v-col cols="12">
        <v-dialog
          v-model="overlay"
          persistent
          max-width="900"
        >
          <v-card>
            <v-card-title class="text justify-center" style="font-size: xx-large;">
              Hello! Thanks for viewing my page!
            </v-card-title>
            <v-spacer></v-spacer>
            <v-card-text class="text" style="font-size: larger;">
              This page is a work in progress, and at the time is best viewed on Desktop browsers.  I am working on making this page fully responsive in my free time.  Thanks for looking!
            </v-card-text>

            <v-card-actions class="justify-center">
              <v-btn
                rounded
                color="success"
                @click="overlay = false, changeLog = true"
              >
              Continue
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>

        <v-dialog
          v-model="changeLog"
          persistent
          max-width="900"
        >
          <v-card>
            <v-card-title class="text justify-center" style="font-size: xx-large;">
              Upcoming Changes:
            </v-card-title>
            <v-spacer></v-spacer>
            <v-card-text class="text" style="font-size: larger;">
              - Minimize/maximize animation for resume dialog. <br/>
              - Change from inline styling (why did I do so much of that???) to dedicated classes
            </v-card-text>

            <v-card-actions class="justify-center">
              <v-btn
                rounded
                color="success"
                @click="overlay = false, changeLog = false"
              >
              Continue To Site
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>

        <v-img
          src="../assets/background.png"
          style="
            height: 100%; 
            width: 100%;
            left: 0%;
            top: 0%;
            position: fixed;"
        > 
          <v-img
            src="../assets/recycle.png"
            style="
            height: 5%; 
            width: 5%;
            left: 0%;
            top: 5%;
            position: fixed;"
          >
          </v-img>

          <v-img
            src="../assets/explorer.png"
            style="
            height: 5%; 
            width: 5%;
            left: 0%;
            top: 13%;
            position: fixed;"
          >
          </v-img>

          <v-img
            src="../assets/computer.png"
            style="
            height: 5%; 
            width: 5%;
            left: 0%;
            top: 21%;
            position: fixed;"
          >
          </v-img>

          <DialogWindow
            v-if="resume"
            :isEnabled="resume"
            :visible.sync="resume"
          >
          </DialogWindow>
          <v-system-bar 
          height="30"
          color="#c0c0c0"
          style="
            left: 0%; 
            bottom: 0%; 
            position: fixed; 
            width: 100%;"
          >
          <v-switch
            v-model="offset"
            label=""
            color="primary"
          ></v-switch>

          <v-menu
            top
            :offset-y="offset"
          >
            <template v-slot:activator="{ on, attrs }">
            <v-btn
              style="
                left: 0%;
                bottom: 0%; 
                position: fixed;
                height: 30px;
                width: 70px;"
                v-bind="attrs"
                v-on="on"
            >
              <v-img
                src="../assets/start.png"
              ></v-img>
            </v-btn>
            </template>

            <v-list
              style="background-color: #c0c0c0; height: 600px; width: 400px;"
            >
              <v-list-item
                v-for="(item, index) in items"
                :key="index"
              >
                <v-list-item-content>
                  <v-list-item-title>
                    <v-hover v-slot="{ hover }">
                      <v-btn
                        text
                        height="50"
                        width="400"
                        @click="item.title == 'Resume' ? openResume() : openLink(item.url)"
                        :style="{ 'background-color': hover ? '#000080' : '#c0c0c0', 'color': hover ? '#ffffff' : '#000000', 'justify-content': 'left' }"
                      >
                        <v-img
                          :src="require(`@/assets/${item.source}.png`)"
                          max-height="50"
                          max-width="50"
                        />
                        {{ item.title }}
                      </v-btn>
                    </v-hover>
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-menu>
         
        <v-container class="trayContainer">
        <v-tooltip top>
          <template v-slot:activator="{ on, attrs }">
            <v-img
              src="../assets/node.png"
              v-bind="attrs"
              v-on="on"
              class="trayContent"
            ></v-img>
          </template>
          <span>Node.js</span>
        </v-tooltip>

        <v-tooltip top>
          <template v-slot:activator="{ on, attrs }">
            <v-img
              src="../assets/vuetify.png"
              v-bind="attrs"
              v-on="on"
              class="trayContent"
            ></v-img>
          </template>
          <span>Vuetify</span>
        </v-tooltip>

        <v-tooltip top>
          <template v-slot:activator="{ on, attrs }">
            <v-img
              src="../assets/vue.png"
              v-bind="attrs"
              v-on="on"
              class="trayContent"
            ></v-img>
          </template>
          <span>Vue</span>
        </v-tooltip>

        <div 
          class="text trayTime"
          >
          {{ timestamp }}
        </div>
      </v-container>
      </v-system-bar>
      </v-img>
      </v-col>
    </v-row>
</v-app>
</template>

<script>
import DialogController from '../data/controllers.js'
import DialogWindow from '../components/templates/dialog.vue'
export default {

name: 'Home',

components: {
  DialogWindow
},

data: () => ({
  //state
  resume: false,
  overlay: true,
  changeLog: false,
  timestamp: "",

  items: [
    { title: 'Resume', source: "documents"},
    { title: 'Github', source: "documents", url: "https://github.com/mattwitter"  },
    { title: 'LinkedIn', source: "documents", url: "https://www.linkedin.com/in/matthewwitter/" },
  ],
  offset: true,
}), 

mounted() {
},

methods: {
  openResume() {
    this.resume = DialogController.resumeDialog(true);
  },

  openLink(url) {
    window.open(url, "_blank", "height=700,width=700");
  },

  closeResume() {
    this.resume = false;
  },

  getNow: function() {
    const today = new Date();
    const date = today.getFullYear()+'-'+(today.getMonth()+1)+'-'+today.getDate();
    const time = today.getHours() + ":" + today.getMinutes();
    const dateTime = date +' '+ time;
    this.timestamp = dateTime;
  }
},
created() {
  setInterval(this.getNow, 1000);
 },

}
</script>

<style scoped>
  .trayContainer {
    display: inline-flex;
    position: absolute;
    border: 1px solid darkslategray;
    width: fit-content;
    height: 30px;
    bottom: 0;
    right: 0;
    justify-content: center; /* Center horizontally */
    align-items: center; /* Center vertically */
    white-space: nowrap;
    text-align: center;
    background-color: darkgray;
  }
  .trayContent {
    display: inline-flex;
    height: 20px;
    width: 30px;
  }
  .trayTime {
    display: inline-flex;
    height: 30px;
    width: auto;
    padding-left: 2px;
  }
</style>