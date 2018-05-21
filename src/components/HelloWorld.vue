<template>
  <div>
    <input type="checkbox" v-model="show">
    <select v-model="src" style="width: 30em">
      <option v-for="item in pdfList" :value="item" v-text="item"></option>
    </select>
    <input v-model.number="page" type="number" style="width: 5em"> /{{numPages}}
    <div class="left-block">
    </div>
    <div id='pdf'>
      <div v-if="loadedRatio > 0 && loadedRatio < 1"
        style="background-color: green; color: white; text-align: center" :style="{ width: loadedRatio * 100 + '%' }">
        {{ Math.floor(loadedRatio * 100) }}%
      </div>
      <pdf v-if="show" ref="pdf" style="border: 1px solid red" :src="src" :page="page" @progress="loadedRatio = $event" @error="error" @num-pages="numPages = $event" @link-clicked="page = $event"></pdf>
    </div>
    <div class="right-block">
      <button class='btn btn-normal'></button>
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
      ], src: '',
      loadedRatio: 0,
      page: 1,
      numPages: 0,
    };
  },
  methods: {
    error(err) {
      console.log(err);
    },
  },
};
</script>

<style>
#pdf {
  width: 30%;
  margin: 0 auto;
}
</style>
