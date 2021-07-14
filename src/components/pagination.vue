<template>
  <div class="hello">
    <VueTailwindPagination
      :current="currentPage"
      :total="total"
      :per-page="perPage"
      @page-changed="onPageClick($event)"
    />
  </div>
  <table class="table table-dark table-striped">
    <thead>
      <tr>
        <th scope="col">Id</th>
        <th scope="col">Email</th>
        <th scope="col">First Name</th>
        <th scope="col">Last Name</th>
        <th scope="col">Avatar</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="d in data" v-bind:key="d.id">
        <td>{{ d.id }}</td>
        <td>{{ d.email }}</td>
        <td>{{ d.first_name }}</td>
        <td>{{ d.last_name }}</td>
        <td><img :src="d.avatar"/></td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import "@ocrv/vue-tailwind-pagination/dist/style.css";
import VueTailwindPagination from "@ocrv/vue-tailwind-pagination";
import axios from "axios";
export default {
  components: {
    VueTailwindPagination,
  },
  data() {
    return {
      currentPage: 0,
      perPage: 0,
      total: 0,
      data: [],
    };
  },
  methods: {
    onPageClick(event) {
      this.currentPage = event;
      this.getData(this.currentPage);
    },
    async getData(pageNumber) {
      var response = await axios.get(
        `https://reqres.in/api/users?page=${pageNumber}`
      );
      var responseData = response.data;
      this.currentPage = responseData.page;
      this.perPage = responseData.per_page;
      this.total = responseData.total;
      this.data = response.data.data;
    },
  },
  mounted(){
    this.currentPage = 1;
    this.getData(this.currentPage);
  }
};
</script>

