<template>
  <div id="app">
    <Login v-if="!user" />
    <template v-else>
      <div class="admin">
        <Header />
      </div>
      <div class="content-wrapper">
        <Sidebar :user="user" />
        <List :serveys="serveyList" />
      </div>
    </template>
  </div>
</template>

<script>
import Header from "../components/layout/Header";
import Sidebar from "../components/layout/Sidebar";
import Login from "../components/Login";
import List from "../components/layout/List";
import EventBus from "../event";
export default {
  name: "Customer",
  components: {
    Header,
    Sidebar,
    List,
    Login
  },
  data() {
    return {
      user: null,
      serveyList: []
    };
  },
  mounted() {
    this.user = localStorage.getItem("user");

    EventBus.$on("userUpdated", () => {
      this.user = localStorage.getItem("user");

      if (this.user) {
        this.initCustomer();
      }
    });

    if (this.user) {
      this.initCustomer();
    }
  },
  methods: {
    initCustomer() {
      this.serveyList = JSON.parse(localStorage.getItem("servey"));
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
    padding-left: 20px;
    margin-left: 15%;
    width: 85%;
    padding-right: 20px;
  }
}
</style>