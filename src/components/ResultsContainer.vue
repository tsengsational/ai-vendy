<template>
    <div class="results-container" >
        <h2 class="results-container__title">Your Inspiration:</h2>
        <div v-show="loading" class="spinner"></div>
        <div v-show="loaded" class="results-image">
            <img :src="inspirationUrl" >
        </div>
    </div>
</template>

<script>
export default {
    name: "ResultsContainer",
    props: ["inspirationUrl", "loading", "loaded", "selectedRoom", "selectedTags"]
}
</script>

<style>
.results-container {
    padding: 3rem 0 0;
}

.results-container__title {
    margin-bottom: 2rem;
}

.results-image img {
    width: 100%;

}

@media (min-width: 50em) {
    .results-image img {
        width: 32rem;
    }
}

.spinner {
  border: 16px solid #f3f3f3; /* Light grey */
  border-top: 16px solid #a8de7d; /* Blue */
  border-radius: 50%;
  width: 120px;
  height: 120px;
  animation: spin 2s linear infinite;
  margin: 2rem auto 0;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>

