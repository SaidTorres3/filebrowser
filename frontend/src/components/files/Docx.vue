<template>
  <div class="docx-container">
    <div class="docx-container__paper">
      <div class="docx-container__paper__content">
        {{ docxtxt }}
      </div>
    </div>
  </div>
</template>

<script>
import { docxToString } from "@/utils/docxToString";
export default {
  props: {
    src: String,
    moveDisabledTime: {
      type: Number,
      default: () => 200,
    },
    classList: {
      type: Array,
      default: () => [],
    },
    zoomStep: {
      type: Number,
      default: () => 0.25,
    },
  },
  data: function () {
    return {
      docxtxt: "",
    };
  },
  mounted() {
    console.log(this.docxtxt);
    this.loadDocument().then((res) => {
      this.docxtxt = res;
    });
  },
  methods: {
    loadDocument: async function () {
      // this.src is the URL to the file. Ex: '/api/files/downloads/Sample4.docx?key=49387441654681&inline=true'
      const docxFile = await fetch(this.src);
      const docxBlob = await docxFile.blob();
      const result = await docxToString(docxBlob);
      return result.toString();
    },
  },
};
</script>

<style>
.docx-container {
  width: 100%;
  height: 100%;
  overflow: auto;
  display: flex;
  justify-content: center;
}

.docx-container__paper {
  background-color: white;
  width: 8.5in;
  min-height: 11in;
  height: max-content;
}

.docx-container__paper__content {
  padding: 0.5in;
  white-space: pre-wrap;
}
</style>
