<template>
  <v-dialog v-model="dialog" max-width="300px" max-height="500px" persistent hide-overlay>
    <div class="windows-explorer" ref="explorerDialog" style="width: 700px; height: 1000px;">
      <!-- Your dialog content here -->
      <v-card-title class="windows-explorer-title" @mousedown.prevent="startDragging">
        <span class="windows-explorer-title-text">{{ title }}</span>
        <div class="custom-dialog-close">
          <v-btn
            text
            x-small
            style="background-color: #c0c0c0; height: 100%"
            class="mb-1 mr-1"
          >
            <v-icon>mdi-window-minimize</v-icon>
          </v-btn>
          <v-btn
            text
            x-small
            style="background-color: #c0c0c0; height: 100%"
            class="mb-1 mr-1"
          >
            <v-icon>mdi-window-maximize</v-icon>
          </v-btn>
          <v-btn
            text
            elevation="24"
            x-small
            style="background-color: #c0c0c0; height: 100%"
            class="mb-1 mr-1"
            @click="close(), resume = false"
          >
            <v-icon>mdi-close-box-outline</v-icon>
          </v-btn>
        </div>
      </v-card-title>
      <v-card-text class="windows-explorer-content">
        <!-- Your dialog content here -->
        <div class="custom-dialog-body">
        <!-- Embedding PDF using iframe -->
        <iframe style="height: 75%" src="https://mwitter-resume-pdf.s3.amazonaws.com/MatthewWitterResume.pdf" class="pdf-viewer"></iframe>
      </div>
      </v-card-text>
    </div>
  </v-dialog>
</template>

<script>
export default {
  data() {
    return {
      dialog: true,
      mouseX: 0,
      mouseY: 0,
      dialogX: 0,
      dialogY: 0,
      title: 'Resume',
    };
  },
  props: {
    visible: {
      type: Boolean,
      required: true,
    },
  },
  methods: {
    startDragging(event) {
      this.mouseX = event.clientX;
      this.mouseY = event.clientY;
      this.dialogX = this.$refs.explorerDialog.offsetLeft;
      this.dialogY = this.$refs.explorerDialog.offsetTop;
      document.addEventListener('mousemove', this.handleMouseMove);
      document.addEventListener('mouseup', this.stopDragging);
    },
    handleMouseMove(event) {
      const deltaX = event.clientX - this.mouseX;
      const deltaY = event.clientY - this.mouseY;
      this.$refs.explorerDialog.style.left = `${this.dialogX + deltaX}px`;
      this.$refs.explorerDialog.style.top = `${this.dialogY + deltaY}px`;
    },
    stopDragging() {
      document.removeEventListener('mousemove', this.handleMouseMove);
      document.removeEventListener('mouseup', this.stopDragging);
    },
    close() {
      this.$emit('update:visible', false);
    }
  }
};
</script>

<style>
.windows-explorer {
  position: fixed;
  background-color: #c0c0c0;
  border: 2px solid #808080;
  border-radius: 6px;
  cursor: default;
}

.windows-explorer-title {
  background-color: #000080;
  color: #fff;
  padding: 4px 8px;
  display: flex;
  align-items: center;
  cursor: default;
  height: 30px;
}

.windows-explorer-title-text {
  position: absolute;
  top: 0;
  justify-content: center;
  font-weight: bold;
  font-size: 16px;
}

.windows-explorer-content {
  padding: 6px;
  cursor: default;
  width: 100%;
  height: 100%;
}

.windows-explorer-actions {
  justify-content: flex-end;
  padding: 8px;
  background-color: #c0c0c0;
  cursor: default;
}

.custom-dialog-background {
  background-color: #c0c0c0;
}

.custom-dialog-close {
  position: absolute;
  justify-content: center;
  cursor: default;
  right: 0;
  margin-right: 2px;
  top: 0;
}

.custom-dialog-body {
  padding: 8px;
  width: 100%;
  height: 100%;
}

.pdf-viewer {
  width: 100%;
  height: 400px; /* Adjust height as needed */
}
</style>
