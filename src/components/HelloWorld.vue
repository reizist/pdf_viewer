<template>
  <div>
    <input type="checkbox" v-model="show">
    <select v-model="src" style="width: 30em">
      <option v-for="item in pdfList" :value="item" v-text="item"></option>
    </select>
    <input v-model.number="page" type="number" style="width: 5em"> /{{numPages}}
    <div class='container'>
      <div class='row main'>
        <div class='col-2'>
          <button type='button' class='btn btn-light btn-control' v-on:click='prevPage()'>prev button</button>
        </div>

        <div class='col-8'>
          <div v-if="loadedRatio > 0 && loadedRatio < 1"
            style="background-color: green; color: white; text-align: center" :style="{ width: loadedRatio * 100 + '%' }">
            {{ Math.floor(loadedRatio * 100) }}%
          </div>
          <pdf v-if="show" ref="pdf" style="border: 1px solid red width: 30%;" :src="src" :page="page" @progress="loadedRatio = $event" @error="error" @num-pages="numPages = $event" @link-clicked="page = $event"></pdf>
        </div>

        <div class='col-2'>
          <button type='button' class='btn btn-light btn-control' v-on:click='nextPage()'>next button</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import pdf from 'vue-pdf';

export default {
  components: {
    pdf,
  },
  data() {
    return {
      show: true,
      pdfList: [
        './static/tracemonkey.pdf',
        './static/sample.pdf',
        './static/sample2.pdf',
      ],
      src: '',
      loadedRatio: 0,
      page: 1,
      numPages: 0,
    };
  },
  methods: {
    error(err) {
      // eslint-disable-next-line
      console.log(err);
    },

    prevPage() {
      this.page = this.page - 1;
    },

    nextPage() {
      this.page = this.page + 1;
    },
  },
};
</script>

<style>
.main {
  height: 600px;
}
.btn-control {
}
</style>
