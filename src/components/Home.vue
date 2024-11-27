<template>
  <div class="home-container">
    <h1 class="title">Algorytm do odkrywania wartości odstającej</h1>
    <p>Podane przez ciebie wartości mogą się składać wyłącznie z liczb parzystych lub nieparzystych, z wyjątkiem jednej liczby która będzie odbiegać od reszty.</p>
    
    <form @submit.prevent="findOutlier">
      <input 
        v-model="numbers" 
        class="input" 
        placeholder="Wpisz liczby po przecinku"
      />
      <div 
        v-if="v$.numbers.$errors.length" 
        class="error-message"
      >
        {{ v$.numbers.$errors[0].$message }}
      </div>
      <button type="submit" class="button">Wyszukaj</button>
    </form>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import { useVuelidate } from '@vuelidate/core'
import { required, helpers } from '@vuelidate/validators'

export default {
  setup() {
    const numbers = ref('')
    
    const rules = computed(() => ({
      numbers: {
        required: helpers.withMessage('Pole jest wymagane', required),
        validNumbers: helpers.withMessage(
          'Wprowadź poprawne liczby całkowite oddzielone przecinkami', 
          (value) => {
            if (!value) return false
            const numArray = value.split(',')
            return numArray.every(num => 
              /^\s*-?\d+\s*$/.test(num.trim())
            )
          }
        ),
        sufficientNumbers: helpers.withMessage(
          'Musisz wprowadzić co najmniej 3 liczby', 
          (value) => {
            if (!value) return false
            const numArray = value.split(',').filter(num => num.trim() !== '')
            return numArray.length >= 3
          }
        )
      }
    }))

    const v$ = useVuelidate(rules, { numbers })

    return { 
      numbers, 
      v$
    }
  },
  methods: {
    async findOutlier() {
      const isValid = await this.v$.$validate()
      
      if (!isValid) return

      const arr = this.numbers.split(",").map(num => Number(num.trim()));
      const isEven = arr.slice(0, 3).filter(num => num % 2 === 0).length > 1;
      const outlier = arr.find(num => (isEven ? num % 2 !== 0 : num % 2 === 0));
      this.$router.push(`/result/${outlier}`);
    }
  }
}
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
  margin-top: 15px;
  margin-bottom: 15px;
  padding: 10px;
  background-color: #ffeeee;
  border: 1px solid #ff9999;
  border-radius: 5px;
  font-size: 14px;
  max-width: 300px;
  margin-left: auto;
  margin-right: auto;
}
</style>