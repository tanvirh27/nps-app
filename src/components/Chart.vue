<template>
  <div class="card">
    <div class="card-img-bottom">
      <chartjs-doughnut :bind="true" :datasets="datasets" :labels="labels" :option="option" />
    </div>
  </div>
</template>

<script>
export default {
  props: ["customerCategories"],
  data() {
    return {
      datasets: [
        {
          data: [],
          backgroundColor: ["#78c38a", "#b9c465", "#e8847c"],
          hoverBackgroundColor: ["#2CC651", "#96A529", "#D35D54"]
        }
      ],
      labels: ["Promoters", "Passives", "Detractors"],
      option: {}
    };
  },
  mounted() {
    this.datasets[0].data = Object.values(this.customerCategories);
  }
};
</script>