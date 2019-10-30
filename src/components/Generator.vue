<template>
  <div id="generator">
    <v-layout column align-center>
      <h2>What is this?</h2>
      <p>これは自分用に作ったプログラムのロゴを作成するジェネレータです。</p>
      <br />
      <h3>Preview</h3>
      <br />
      <v-card width="512" height="512" id="preview" ref="output">
        <p class="preview-font">{{ text }}</p>
      </v-card>
      <br />
      <h3>Color</h3>
      <br />
      <v-row justify="space-around">
        <v-color-picker v-model="colors.left" class="ma-2"></v-color-picker>
        <v-color-picker v-model="colors.right" class="ma-2"></v-color-picker>
      </v-row>
      <br />
      <h3>Inputs</h3>
      <br />
      <v-row justify="space-around">
        <v-text-field class="ma-2" label="Icon text" type="string" v-model="text"></v-text-field>
        <v-text-field class="ma-2" label="Font size" type="number" v-model="fontSize"></v-text-field>
      </v-row>
      <v-btn @click="download">download</v-btn>
    </v-layout>
    <component :is="style"></component>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from "vue-property-decorator";
import { CreateElement } from "vue";
const FileSaverAsync = () => import("file-saver");
const domtoimgAsync = () => import("dom-to-image");

@Component
export default class Generator extends Vue {
  public colors = {
    left: "#e66465",
    right: "#9198e5"
  };

  public text = "Vexera";

  public fontSize = 100;

  get style() {
    return {
      render: (html: CreateElement) => {
        return html("style", {
          domProps: {
            innerHTML: `
              #preview {
                background: linear-gradient(45deg, ${this.colors.left}, ${this.colors.right});
              }

              .preview-font {
                font-size: ${this.fontSize}px;
              }
            `
          }
        });
      }
    };
  }

  public async download() {
    const saver = await FileSaverAsync();
    const domtoimg = await domtoimgAsync().then(value => value.default);
    domtoimg
      .toBlob(document.getElementById("preview") as Node, {
        width: 512,
        height: 512
      })
      .then((blob: Blob) => {
        saver.saveAs(blob, "icon.png");
      });
  }
}
</script>

<style lang="scss">
#preview {
  text-align: center;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  display: flex;
}
</style>