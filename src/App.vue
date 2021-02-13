<template>
    <div id="app">
        <button @click="downloadPdf">Download pdf</button>
        <span class="label-progress">{{ progress }}</span>

        <vue-html2pdf
            :show-layout="false"
            :float-layout="true"
            :enable-download="true"
            :preview-modal="true"
            :filename="'taaha'"
            :paginate-elements-by-height="1100"
            :pdf-quality="2"
            :pdf-format="'a4'"
            :pdf-orientation="'portrait'"
            :pdf-content-width="'100vw'"
            :manual-pagination="false"
            :html-to-pdf-options="{
        margin: 0,
        filename: 'hee hee.pdf',
        image: {
          type: 'jpeg',
          quality: 0.98,
        },
        enableLinks: true,
        html2canvas: {
          scale: 2,
          useCORS: true,
        },
        jsPDF: {
          unit: 'in',
          format: 'a4',
          orientation: 'portrait',
        },
      }"
            @progress="onProgress($event)"
            @startPagination="startPagination()"
            @hasPaginated="hasPaginated()"
            @beforeDownload="beforeDownload($event)"
            @hasDownloaded="hasDownloaded($event)"
            ref="html2Pdf"
        >
            <pdf-content slot="pdf-content"/>
        </vue-html2pdf>

    </div>
</template>

<script>
import PdfContent from "@/components/PdfContent";

import VueHtml2pdf from 'vue-html2pdf'

export default {
    name: "app",

    data() {
        return {
            contentRendered: false,
            progress: 0,
            generatingPdf: false,
            pdfDownloaded: false,
        };
    },

    methods: {
        async downloadPdf() {
            this.$refs.html2Pdf.generatePdf();
        },

        onProgress(progress) {
            this.progress = progress;
            console.log(`PDF generation progress: ${progress}%`)
        },

        startPagination() {
            console.log(`PDF has started pagination`)
        },

        hasPaginated() {
            console.log(`PDF has been paginated`)
        },

        async beforeDownload({html2pdf, options, pdfContent}) {
            console.log(`On Before PDF Generation`)
        },

        hasDownloaded(blobPdf) {
            console.log(`PDF has downloaded yehey`)
            this.pdfDownloaded = true
            console.log(blobPdf)
        },
    },

    components: {
        VueHtml2pdf,
        PdfContent,

    },
};
</script>

<style lang="scss">

</style>
