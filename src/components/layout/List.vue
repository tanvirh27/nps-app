<template>
  <div class="admin-content customer-list">
    <h3>All Customer</h3>
    <table>
      <tr>
        <th>Customer Name</th>
        <th>Customer Email</th>
        <th>Product Score</th>
        <th>Service Score</th>
        <th>Comment</th>
        <th>Date</th>
      </tr>
      <tr :key="servey.createdAt" v-for="servey in serveys">
        <td>{{ servey.clientName}}</td>
        <td>{{ servey.clientEmail}}</td>
        <td>{{ calculateScore(servey.productScore) }}</td>
        <td>{{ servey.serviceScore}}</td>
        <td>{{ servey.suggestion}}</td>
        <td>{{ chageDateFormate(servey.createAt) }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: "List",
  props: ["serveys"],
  methods: {
    /**
     * this method is responsible calculate customer metrics
     *
     */
    calculateScore(score) {
      let status;
      if (score > 8) status = "Promoters";
      if (score > 5 && score <= 8) status = "Passives";
      if (score < 6) status = "Detractors";
      return status;
    },

    /**
     * this method is responsible for formatting date
     *
     */
    chageDateFormate(createDate) {
      return new Date(createDate).toISOString().split("T")[0];
    }
  }
};
</script>

<style lang="scss">
.customer-list {
  margin-top: 30px;
}
</style>