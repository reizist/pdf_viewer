<template>
  <div>
    <select v-model="src" style="width: 30em">
      <option v-for="item in pdfList" :value="item" v-text="item"></option>
    </select>
    <input v-model.number="page" type="number" style="width: 5em"> /{{numPages}}
    <div class='container'>
      <div class='row main'>
        <div class='col-2'>
          <button type='button' class='btn btn-light btn-control' v-on:click='prevPage'>prev page</button>
        </div>

        <div class='col-8'>
          <div class="progress" style='height: 2px;'>
            <div class='progress-bar bg-success' role='progressbar' v-if="loadedRatio > 0 && loadedRatio < 1"
              v-bind:aria-valuenow="loadedRatio * 100" aria-valuemin="0" aria-valuemax="100" :style="{ width: loadedRatio * 100 + '%' }">
            </div>
          </div>
          <pdf ref="pdf" style="border: 1px solid red width: 30%;"
            :src="src"
            :page="page"
            @progress="loadedRatio = $event"
            @error="error"
            @num-pages="numPages = $event"
            @link-clicked="page = $event"
            @loaded="resetPage()">
          </pdf>
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

    resetPage() {
      this.page = 1;
    },

    prevPage() {
      if (this.page > 1) {
        this.page = this.page - 1;
      }
    },

    nextPage() {
      if (this.page < this.numPages) {
        this.page = this.page + 1;
      }
    },
  },

  created: function () {
    var vm = this;
    window.addEventListener('keydown', function(event) {
      switch (event.key) {
        case "ArrowLeft":
          vm.prevPage();
          break;
        case "ArrowRight":
          vm.nextPage();
          break;
      }
    });
  },
};
</script>

<style>
.main {
  height: 600px;
}
.btn-control {
}

.progress {
  margin: 3px 0;
}
</style>
