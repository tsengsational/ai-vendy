<template>
  <div class="interior-desaign">
    <h2 class="interior-desaign__title">AI Vendy</h2>
    <DesignForm
      :submitPrompt="submitPrompt"
      @resetForm="resetForm" />
    <results-container
      v-show="submitted"
      :inspirationUrl="inspirationUrl"
      :loaded="loaded"
      :loading="loading"
      :selectedType="selectedType"
      :selectedTags="selectedTags" />
  </div>
</template>

<script>
import DesignForm from './components/DesignForm.vue';
import ResultsContainer from './components/ResultsContainer.vue';

export default {
  name: 'App',
  components: {
    DesignForm,
    ResultsContainer
  },
  data() {
    return {
      inspiration: null,
      submitted: false,
      selectedType: null,
      selectedTags: null
    }
  },
  computed: {
    inspirationUrl() {
      if (this.inspiration) {
        return this.inspiration.data[0].url
      }
      else {
        return ""
      }
    },
    loading() {
      return this.submitted && !this.inspiration;
    },
    loaded() {
      return this.submitted && this.inspirationUrl.length > 1;
    }
  },
  methods: {
    resetForm() {
      this.selectedType = null;
      this.selectedTags = null;
      this.inspiration = null;
      this.submitted = null;
    },
    async submitPrompt(textPrompt, selectedType, selectedTags) {
      const payload = {
        prompt: textPrompt,
        n: 1,
        size: "512x512"
      }
      this.selectedType = selectedType;
      this.selectedTags = selectedTags;
      this.submitted = true;

      const response = await fetch('https://api.openai.com/v1/images/generations', {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          "Authorization": `Bearer ${process.env.VUE_APP_OPENAI_API_KEY}`
        },
        body: JSON.stringify(payload)
      })
      let fulfilledResponse = await response.json()
      this.inspiration = await fulfilledResponse
    }
  }
}
</script>

<style>
.interior-desaign__title {
  font-size: 2rem;
  font-weight: bold;
}

#app {
    background-color: #f0f7e3;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin: 60px auto;
    max-width: 50rem;
    padding: 2rem 0;
}
</style>
