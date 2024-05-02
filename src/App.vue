<template>
  <div class="container mx-auto">
    <div class="text-center">
      <h1 class="text-3xl">KÆRLIGHEDSREGNEMASKINEN</h1>
      <form @submit.prevent="calculateLove" class="mt-8">
        <div class="input-container">
          <label for="yourName" class="font-bold text-xl block">DIT NAVN:</label>
          <input type="text" id="yourName" v-model="yourName" required class="input-field mb-4">
        </div>
        <div class="input-container">
          <label for="partnerName" class="font-bold text-xl block">DIT CRUSH NAVN:</label>
          <input type="text" id="partnerName" v-model="partnerName" required class="input-field mb-4">
        </div>
        <button type="submit" class="px-10 py-4 text-xl bg-green-500 text-white rounded-lg cursor-pointer transition-colors hover:bg-green-600">UDREGN</button>
      </form>
      <div v-if="lovePercentage !== null" class="mt-8">
        <div class="font-bold text-2xl">{{ yourName }} og {{ partnerName }} har en kærlighedsprocent på:</div>
        <div class="text-2xl">{{ lovePercentage }}%</div>
      </div>
      <div v-if="lovePercentage !== null" class="mt-8">
        <div class="w-full h-16 bg-gray-300 rounded-full overflow-hidden">
          <div :style="{ width: progressBarWidth }" class="progress-bar h-full text-xl text-white text-center bg-green-500 rounded-full">{{ lovePercentage }}%</div>
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
      lovePercentage: null
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
      } catch (error) {
        console.error(error);
      }
    }
  }
};
</script>

<style>
.progress-bar {
  transition: width 0.3s ease-in-out;
}

.input-container {
  width: 500px; /* Adjust the width as per your preference */
  margin: 0 auto 1rem; /* Center horizontally and add bottom margin */
}

.input-field {
  width: 100%; /* Make input fields fill their container */
  padding: 0.75rem; /* Add padding for better appearance */
  font-size: 1rem; /* Adjust font size if needed */
  border: 2px solid #ccc; /* Add border */
  border-radius: 0.5rem; /* Add border radius for rounded corners */
  box-sizing: border-box; /* Ensure padding and border are included in width */
}
</style>
