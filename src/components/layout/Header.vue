<template>
  <header>
    <div class="container">
      <div class="logo">
        <a href>NPS Portal</a>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: "Header"
};
</script>

<style lang='scss'>
.admin {
  header {
    overflow: hidden;
    position: fixed;
    top: 0;
    width: 100%;
    .container {
      width: 100%;
      .logo {
        padding-left: 30px;
      }
    }
  }
}
</style>