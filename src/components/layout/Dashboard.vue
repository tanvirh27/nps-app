<template>
  <div class="admin-content">
    <h2 class="admin-title">Dashboard</h2>
    <div class="servey-category">
      <div v-bind:key="category" v-for="(value, category) in customerCategories">
        <h3>{{value}}</h3>
        <p>{{category}}</p>
      </div>
    </div>

    <div class="info">
      <div class="graph-wrapper">
        <h3>Product Satisfaciton</h3>
        <div>
          <ChartDoughnut v-if="loaded" :customerCategories="customerCategories" />
        </div>
      </div>
      <div class="info-item">
        <h3>Last 7 days Customer</h3>
        <table>
          <tr>
            <th>Customer Name</th>
            <th>Customer Email</th>
            <th>Product Score</th>
            <th>Service Score</th>
            <th>Date</th>
          </tr>
          <tr :key="servey.createdAt" v-for="servey in filteredData">
            <td>{{ servey.clientName }}</td>
            <td>{{ servey.clientEmail }}</td>
            <td>{{ calculateScore(servey.productScore) }}</td>
            <td>{{ servey.serviceScore }}</td>
            <td>{{ chageDateFormate(servey.createAt) }}</td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import ChartDoughnut from "@/components/Chart";
export default {
  name: "Dashboard",
  props: ["customerCategories", "serveyList"],
  components: {
    ChartDoughnut
  },

  data() {
    return {
      filteredData: [],
      loaded: false
    };
  },

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
  },
  mounted() {
    this.$nextTick(() => {
      this.loaded = true;
    });

    // get last 7 days servey
    let lastSevenDays = 24 * 60 * 60 * 1000 * 7;
    let currentTime = new Date().getTime();
    lastSevenDays = currentTime - lastSevenDays;
    this.filteredData = this.filteredData = this.serveyList.filter(function(e) {
      return e.createAt > lastSevenDays;
    });
  }
};
</script>

<style lang='scss'>
.servey-category {
  margin-top: 15px;
  display: flex;
  flex-wrap: wrap;
  div {
    margin-bottom: 10px;
    margin-right: 20px;
    padding: 0 30px;
    color: #fff;
    background: #78c38a;
    height: 140px;
    width: 256px;
    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.12), 0px 2px 2px rgba(0, 0, 0, 0.24),
      0px 0px 2px rgba(0, 0, 0, 0.12);
    border-radius: 5px;
    h3 {
      font-weight: bold;
      font-size: 48px;
      line-height: 0;
    }
    p {
      padding-left: 5px;
      font-weight: 300;
      font-size: 24px;
      line-height: 0;
      text-transform: capitalize;
    }
    &:nth-child(2) {
      background-color: #b9c465;
    }
    &:nth-child(3) {
      background-color: #e8847c;
    }
  }
}

.info {
  display: flex;
  flex-wrap: wrap;
  padding-top: 30px;
  .graph-wrapper {
    width: 40%;
  }
  .info-item {
    width: 50%;
  }
}
.admin-title {
  font-size: 22px;
  margin-top: 40px;
}
</style>