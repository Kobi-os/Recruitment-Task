<template>
  <div class="home-container">
    <h1 class="title">Algorytm do odkrywania wartości odstającej</h1>
    <p>Podane przez ciebie wartości mogą się składać wyłącznie z liczb parzystych lub nieparzystych, z wyjątkiem jednej liczby która będzie odbiegać od reszty.
    </p>
    <input v-model="numbers" class="input" placeholder="Wpisz liczby po przecinku" />
    <button class="button" @click="findOutlier">Wyszukaj</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      numbers: ""
    };
  },
  methods: {
    findOutlier() {
      const arr = this.numbers.split(",").map(Number);
      const isEven = arr.slice(0, 3).filter(num => num % 2 === 0).length > 1;
      const outlier = arr.find(num => (isEven ? num % 2 !== 0 : num % 2 === 0));
      this.$router.push(`/result/${outlier}`);
    }
  }
};
</script>

<style scoped>
.home-container {
  text-align: center;
  justify-content: center;
  align-items: center;
  margin-top: 50px;
  font-family: "Verdana";
}

.title {
  font-size: 24px;
  color: #333;
}

.input {
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 300px;
  margin-right: 10px;
}

.button {
  padding: 10px 20px;
  font-size: 16px;
  color: white;
  background-color: #007bff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.button:hover {
  background-color: #0056b3;
}
</style>
