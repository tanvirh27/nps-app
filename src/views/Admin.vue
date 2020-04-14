<template>
  <div id="app">
    <Login v-if="!user" />
    <template v-else>
      <div class="admin">
        <Header />
      </div>
      <div class="content-wrapper">
        <Sidebar :user="user" />
        <Dashboard :customerCategories="customerCategories" :serveyList="serveyList" />
      </div>
    </template>
  </div>
</template>

<script>
import Header from "../components/layout/Header";
import Sidebar from "../components/layout/Sidebar";
import Dashboard from "../components/layout/Dashboard";
import Login from "../components/Login";
import EventBus from "../event";
export default {
  name: "Admin",
  components: {
    Header,
    Sidebar,
    Dashboard,
    Login
  },
  data() {
    return {
      user: null,
      serveyList: [],
      customerCategories: {
        promoters: "",
        passives: "",
        detractors: ""
      },
      totalServey: ""
    };
  },
  mounted() {
    this.user = localStorage.getItem("user");

    EventBus.$on("userUpdated", () => {
      this.user = localStorage.getItem("user");

      if (this.user) {
        this.initDashboard();
      }
    });

    if (this.user) {
      this.initDashboard();
    }
  },
  methods: {
    initDashboard() {
      this.serveyList = JSON.parse(localStorage.getItem("servey"));

      this.customerCategories.promoters = this.serveyList.reduce(
        (total, servey) => {
          if (servey.productScore > 8) {
            total++;
          }

          return total;
        },
        0
      );

      // total passives
      this.customerCategories.passives = this.serveyList.reduce(
        (total, servey) => {
          if (servey.productScore > 6 && servey.productScore <= 8) {
            total++;
          }

          return total;
        },
        0
      );

      // total detractors
      this.customerCategories.detractors = this.serveyList.reduce(
        (total, servey) => {
          if (servey.productScore <= 6) {
            total++;
          }

          return total;
        },
        0
      );
    }
  }
};
</script>


<style lang="scss">
.content-wrapper {
  display: flex;

  .admin-sidebar {
    margin-top: 57px;
    display: block;
    width: 15%;
    background-color: #302247;
    height: 100%;
    position: fixed;
    top: 0;
    left: 0;
    overflow-x: hidden;
    padding-top: 20px;
    .admin-menu {
      padding: 0;
      margin: 0;
      li {
        font-weight: 300;
        list-style: none;
        font-size: 14px;
        line-height: 21px;
        padding: 20px 30px 20px;
        border-bottom: 1px solid #333333;
        a {
          color: #fff;
        }
      }
    }
  }

  .admin-content {
    padding-top: 30px;
    padding-left: 20px;
    margin-left: 15%;
    width: 85%;
    padding-right: 20px;
  }
}
</style>