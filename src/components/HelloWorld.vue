<template>
  <div>
    <div class="jumbotron">
      <h2>ファイル詳細</h2>
      <ul>
        <li>名前: {{file.name}}</li>
        <li>サイズ: {{ file.size }} bytes</li>
        <li>種類: {{file.type}}</li>
      </ul>
    </div>

    <input @change="file_change" ref="file" type="file" accept="pdf/*" />
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
      src: '',
      loadedRatio: 0,
      page: 1,
      numPages: 0,
      file: '',
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

    file_change(event) {
      const elFile = this.$refs.file;
      this.file = elFile.files[0];
      console.log(this.file);
      const url = new FileReader().readAsDataURL(this.file);
      this.src = url;
      console.log('file changed: ' + this.src);
    },
  },

  created() {
    const vm = this;
    window.addEventListener('keydown', () => {
      switch (event.key) {
        case 'ArrowLeft':
          vm.prevPage();
          break;
        case 'ArrowRight':
          vm.nextPage();
          break;
        default:
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
