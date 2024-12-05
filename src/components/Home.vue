<template>
  <div class="home-container">
    <h1 class="title">Algorytm do odkrywania wartości odstającej</h1>
    <p>Podane przez ciebie wartości mogą się składać wyłącznie z liczb parzystych lub nieparzystych, z wyjątkiem jednej liczby która będzie odbiegać od reszty.</p>
    
    <input 
      v-model="numbers" 
      class="input" 
      placeholder="Wpisz liczby po przecinku" 
    />
    
    <button 
      class="button" 
      @click="findOutlier"
    >
      Wyszukaj
    </button>

    <p v-if="errorMessage" class="error-message">
      {{ errorMessage }}
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      numbers: "",
      errorMessage: ""
    };
  },
  methods: {
    findOutlier() {
      // Resetuj poprzedni komunikat o błędzie
      this.errorMessage = "";

      // Konwersja i oczyszczenie wprowadzonych liczb
      const arr = this.numbers.split(",")
        .map(num => Number(num.trim()))
        .filter(num => !isNaN(num));

      // Sprawdzenie, czy wprowadzono liczby
      if (arr.length === 0) {
        this.errorMessage = "Proszę wprowadzić liczby.";
        return;
      }

      // Sprawdzenie, czy są zarówno liczby parzyste, jak i nieparzyste
      const evens = arr.filter(num => num % 2 === 0);
      const odds = arr.filter(num => num % 2 !== 0);

      if (evens.length === 0 || odds.length === 0) {
        this.errorMessage = "Proszę wprowadzić co najmniej jedną liczbę parzystą i jedną nieparzystą.";
        return;
      }

      // Logika znajdowania wartości odstającej
      const isEven = evens.length === 1;
      const outlier = isEven ? evens[0] : odds[0];
      
      // Nawigacja do strony wyniku
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

.error-message {
  color: red;
  margin-top: 10px;
  font-size: 14px;
}
</style>