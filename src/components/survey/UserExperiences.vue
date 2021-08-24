<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadData">Load Submitted Experiences</base-button>
      </div>
      <p v-if="isLoading === true">Loading...</p>
      <p v-else-if="!isLoading && error">{{ error }}</p>
      <p v-else-if="isLoading === false && results.length === 0">
        No Stored Data Found! Start Adding Some Data First
      </p>
      <ul v-else-if="isLoading === false && results.length > 0">
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
import axios from 'axios';

export default {
  // props: ['results'],
  components: {
    SurveyResult
  },
  data() {
    return {
      results: [],
      isLoading: false,
      error: null
    };
  },
  methods: {
    loadData() {
      //   fetch(
      //     'https://vue-http-demo-6b251-default-rtdb.asia-southeast1.firebasedatabase.app/surveys.json'
      //   )
      //     .then(response => {
      //       if (response.ok) {
      //         return response.json();
      //       }
      //     })
      //     .then(data => {
      //       console.log(data);
      //       const res = [];
      //       for (const id in data) {
      //         res.push({
      //           id: id,
      //           name: data[id].name,
      //           rating: data[id].rating
      //         });
      //       }
      //       this.results = res;
      //     });
      // },
      this.isLoading = true;
      this.error = null;
      axios
        .get(
          'https://vue-http-demo-6b251-default-rtdb.asia-southeast1.firebasedatabase.app/surveys.json' //firebase need vpn
        )
        .then(response => {
          // then structure : .then((...) => {...})
          this.isLoading = false;
          const data = response.data;
          console.log(data);
          const res = [];
          for (const id in data) {
            res.push({
              id: id,
              name: data[id].name,
              rating: data[id].rating
            });
          }
          this.results = res;
        })
        //one way to handle error
        .catch(() => {
          this.isLoading = false;
          this.error = 'failed to access data! please try again later.';
        });
    },
    mounted() {
      console.log('mounted');
      this.loadData();
    }
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
