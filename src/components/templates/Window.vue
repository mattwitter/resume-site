<template>
  <v-container v-if="isEnabled">
    <v-col cols="3">
    <v-system-bar
      color="#817a82"
      window
    >
    <v-row style="display: flex; justify-content: end;">
      <p class="text mr-8 mt-3">RESUME - MATTHEW WITTER</p>
      <v-icon>mdi-minus</v-icon>
      <v-icon>mdi-checkbox-blank-outline</v-icon>
      <v-btn
        text
        x-small
        style="padding: 0px; margin-left: -10px; margin-top: 17px;"
        @click="closeResume()"
      >
      <v-icon>mdi-close</v-icon>
      </v-btn>
    </v-row>
    </v-system-bar>

    <v-card
    class="mx-auto"
    width="max"
    color="#c0c0c0"
    :height=height
    elevation="10"
    >
    </v-card>
    </v-col>
  </v-container>
</template>

<script>
import DialogController from '../../data/controllers.js'
export default {
  name: "Window",

  data: () => ({
  //state
  resume: false,

}),

mounted() {
},

methods: {
  closeResume() {
    this.isEnabled = DialogController.resumeDialog(false)
    console.log(this.resume)
  }
},

  props: {
    height: {
      type: Number,
      required: true
    },
    isEnabled: {
      type: Boolean,
      required: true
    }
  },
}
</script>