<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences">Load Submitted Experiences</base-button>
      </div>
      <p v-if="isLoading">Loading data...</p>
      <p v-else-if="!isLoading && (!results || results.length === 0)">No stored Experiences found. Please add some!</p>
      <ul v-else-if="!isLoading && results && results.length > 0">
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
      isLoading: false,
    }
  },
  methods: {
    loadExperiences() {
      this.isLoading = true;
      fetch(`${process.env.VUE_APP_FIREBASE_API}/surveys.json`).then((response) => {
        if(response.ok) {
          return response.json()
        }
      }).then((data) => {
        const results = [];
        for(const id in data) {
          results.push({ id, name: data[id].name, rating: data[id].rating });
        }
        this.results = results;
        this.isLoading = false;
      })
    }
  },
  mounted() {
    this.loadExperiences();
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