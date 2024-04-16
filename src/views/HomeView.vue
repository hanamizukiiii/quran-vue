<template>
  <main class="bg-gray-100 min-h-screen py-10">
    <h1 class="text-center font-bold text-4xl text-gray-800 mb-8">Online Qur'an</h1>
    <div class="flex justify-center items-center mb-8">
      <input
        v-model="searchTerm"
        type="text"
        placeholder="Search Surah by Latin Name"
        class="p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
      />
    </div>
    <div class="flex justify-center items-center h-[80vh]" v-if="!data">
      <img class="h-16 w-16" src="https://icons8.com/preloaders/preloaders/1488/Iphone-spinner-2.gif" alt="Loading...">
    </div>
    <div class="container mx-auto" v-else>
      <div class="grid grid-cols-1 mx-5 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
        <div v-for="item in filteredData" :key="item.nomor">
          <div class="bg-white p-6 rounded-lg shadow-md hover:shadow-lg transition duration-300">
            <router-link :to="'/surat/' + item.nomor" class="block">
              <h2 class="text-lg font-semibold text-gray-800">{{ item.nama }}</h2>
              <p class="text-gray-600">{{ item.namaLatin }}</p>
              <p class="text-sm text-gray-700 mt-2">Number of Verses: {{ item.jumlahAyat }}</p>
            </router-link>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>


<script>
import axios from 'axios';

export default {
  data() {
    return {
      data: null,
      searchTerm: '',
    };
  },
  computed: {
    filteredData() {
      if (!this.data) return null;

      const searchTermLowerCase = this.searchTerm.toLowerCase();
      return this.data.filter((item) =>
        item.namaLatin.toLowerCase().includes(searchTermLowerCase)
      );
    },
  },
  async created() {
    const response = await axios.get("https://equran.id/api/v2/surat");
    this.data = response.data.data;
    console.table({this.data});
  },
};
</script>
