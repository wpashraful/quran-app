<script>
import axios from "axios";

export default {
  name: 'App',
  data(){
    return {
      suras: [

      ],
      currentsura: [],
      loading: true
    }

  },
  methods:{
    getsuranumber(e){
        console.log(e.target.value)
      this.querysurahname(e.target.value)
    },
    querysurahname(suranumber){
      this.loading = true
      axios.get('https://api.alquran.cloud/v1/surah/' + suranumber)
          .then(response => {
            this.currentsura = response.data.data})
          this.loading = false
    }
  },
  mounted() {
    axios.get('https://api.alquran.cloud/v1/surah')
        .then(response => {
          console.log(response.data.data)
          this.suras = response.data.data
        })
       this.querysurahname(1)

        // .then(response => console.log(response.data.data))

  }

}
</script>

<template>
    <div class="min-h-screen bg-gray-100 ">
      <div class="container mx-auto rounded">
        <div class="bg-white shadow-lg">
          <div class="flex item-center justify-center p-4 ayah ">
             <div class="flex-1 p-2 font-bold text-xl arrowp ">
               <select @change="getsuranumber"  class=" arabic-text arrow border border-gray-300 text-gray-900 text-2xl rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500 py-5 px-auto">
                 <option  v-for="sura in suras" :value="sura.number">{{ sura.name }} {{ sura.englishName }}</option>
               </select>
             </div>
            <div class="flex-1 text-4xl font-bold flex-row item-center justify-center px-6">
                  <h1>{{ currentsura.name }}</h1>
                  <h1 >{{ currentsura.englishName }}</h1>
            </div>

          </div>
          <div v-if="loading" class="flex">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 animate-spin">
              <path stroke-linecap="round" stroke-linejoin="round" d="M4.5 12a7.5 7.5 0 0015 0m-15 0a7.5 7.5 0 1115 0m-15 0H3m16.5 0H21m-1.5 0H12m-8.457 3.077l1.41-.513m14.095-5.13l1.41-.513M5.106 17.785l1.15-.964m11.49-9.642l1.149-.964M7.501 19.795l.75-1.3m7.5-12.99l.75-1.3m-6.063 16.658l.26-1.477m2.605-14.772l.26-1.477m0 17.726l-.26-1.477M10.698 4.614l-.26-1.477M16.5 19.794l-.75-1.299M7.5 4.205L12 12m6.894 5.785l-1.149-.964M6.256 7.178l-1.15-.964m15.352 8.864l-1.41-.513M4.954 9.435l-1.41-.514M12.002 12l-3.75 6.495" />
            </svg>
             ...loading
          </div>
<!--        <div v-else >-->
          <div v-else v-if="currentsura.hasOwnProperty('ayahs')" v-for="ayah in currentsura.ayahs" class="ayah flex items-center mx-4 border border-gray-100 mb-1 p-5">
            <span  class="text-center block px-6 opacity-70 icon text-2xl p-5 font-bold" >{{ ayah.numberInSurah }}</span>
            <p class="text-right text-3xl text-gray-800  dark:text-gray-400 px-6 arabic-text font">{{ayah.text}}</p>
          </div>
<!--        </div>-->

        </div>
      </div>
    </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Noto+Naskh+Arabic:wght@400;500;600&display=swap');
</style>
