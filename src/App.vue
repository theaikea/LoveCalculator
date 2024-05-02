<template>
  <div class="container mx-auto">
    <div class="text-center">
      <h1 class="text-5xl mt-36">KÆRLIGHEDSREGNEMASKINEN</h1>
      <form @submit.prevent="calculateLove" class="mt-8">
        <div class="input-container">
          <label for="yourName" class="font-bold text-xl block">DIT NAVN:</label>
          <input type="text" id="yourName" v-model="yourName" required class="input-field mb-4">
        </div>
        <div class="input-container">
          <label for="partnerName" class="font-bold text-xl block">DIT CRUSH NAVN:</label>
          <input type="text" id="partnerName" v-model="partnerName" required class="input-field mb-4">
        </div>
        <button type="submit" class="px-10 py-4 text-xl bg-pink-500 text-white rounded-lg cursor-pointer transition-colors hover:bg-pink-600">UDREGN</button>
      </form>
      <div class="mt-8">
        <div v-if="showLovePercentage" class="font-bold text-2xl">{{ yourName }} og {{ partnerName }} har en kærlighedsprocent på:</div>
        <div class="text-2xl">{{ lovePercentage }}%</div>
        <div class="w-full h-16 bg-gray-300 rounded-full mt-4">
        
          <div :style="{ width: progressBarWidth }" class="progress-bar h-full text-xl text-white text-center bg-pink-500 rounded-full"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      yourName: '',
      partnerName: '',
      lovePercentage: null,
      showLovePercentage: false
    };
  },
  computed: {
    progressBarWidth() {
      return this.lovePercentage !== null ? this.lovePercentage + '%' : '0%';
    }
  },
  methods: {
    async calculateLove() {
      const options = {
        method: 'GET',
        url: 'https://love-calculator.p.rapidapi.com/getPercentage',
        params: {
          sname: this.yourName,
          fname: this.partnerName
        },
        headers: {
          'X-RapidAPI-Key': 'a9b7bdae8cmsh44b79676c4af569p1cf460jsneb184dfbcc45',
          'X-RapidAPI-Host': 'love-calculator.p.rapidapi.com'
        }
      };

      try {
        const response = await axios.request(options);
        this.lovePercentage = response.data.percentage;
        this.showLovePercentage = true; 
      } catch (error) {
        console.error(error);
      }
    }
  }
};
</script>

<style>
.progress-bar {
  transition: width 0.5s ease-in-out;
}

.input-container {
  width: 500px;
  margin: 0 auto 1rem;
}

.input-field {
  width: 100%;
  padding: 0.75rem;
  font-size: 1rem;
  border: 2px solid #ccc;
  border-radius: 0.5rem;
  box-sizing: border-box;
}


</style>
