<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="getData">Load Submitted Experiences</base-button>
      </div>
      <p v-if="status">Loading ...</p>
      <p v-else-if="!status && results.length === 0 && !error">No data experiences found. Start adding some survey</p>
      <p v-else-if="!status && error">{{error}}</p>
      <ul v-else-if="!status && results.length > 0">
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      status: false,
      error: null
    }
  },
  methods: {
    getData() {
      this.status = true;
      fetch('https://vue-tryhttp-default-rtdb.asia-southeast1.firebasedatabase.app/surveys.json')
      .then((responses) => {
        // responses.ok ok nya merupakan fungsi js untuk mengecek apakh data berhasil diget atau tidak
        if(responses.ok) {
          // jika data berhasil didapatkan maka me return data api menjadi bentuk json
          return responses.json();
        }
      })
      .then((data) => {
        this.status = false
        // console.log(data);
        // membuat variabel array agar sesuai dengan data variabel this.results
        const results = [];
        for(const id in data) {
          // kita memasukkan data database kedalam variabel results
          results.push({
          // ini merupakan data yang di masukkan
          id: id,
          name: data[id].name,
          rating: data[id].rating,
          })
        }
        // lalu kita masukkan data yang tadi telah diget kedalam data kita this.results
        this.results = results;
      })
      .catch((error) => {
        this.status = false;
        this.error = error;
      })
    },
  },
  mounted() {
    this.getData();
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>