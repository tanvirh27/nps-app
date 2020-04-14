<template>
  <div id="app">
    <Header />
    <NpsForm />
  </div>
</template>

<script>
import Header from "../components/layout/Header";
import NpsForm from "../components/NpsForm";

export default {
  name: "Home",
  components: {
    Header,
    NpsForm
  }
};
</script>

