<template>
  <div class="result-container">
    <h1 class="title">Wartość odstająca</h1>
    <p class="result">{{ outlier }}</p>
    <button class="button" @click="goBack">Powrót</button>
  </div>
</template>

<script>
export default {
  computed: {
    outlier() {
      return this.$route.params.outlier;
    }
  },
  methods: {
    goBack() {
      this.$router.push("/");
    }
  }
};
</script>

<style scoped>
.result-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: white;
  font-family: "Verdana";
}

.title {
  font-size: 28px;
  font-weight: bold;
  color: #333;
  margin-bottom: 20px;
}

.result {
  font-size: 48px;
  color: #007bff;
  font-weight: bold;
  margin-bottom: 40px;
}

.button {
  padding: 10px 20px;
  font-size: 16px;
  color: white;
  background-color: #007bff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.button:hover {
  background-color: #0056b3;
}
</style>
