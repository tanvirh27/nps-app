<template>
  <div id="app">
    <router-view />
  </div>
</template>

<style lang="scss">
/* common style */
html,
body {
  font-size: 16px;
  margin: 0;
  padding: 0;
  font-family: "Roboto", sans-serif;
  color: #232323;
  height: 100%;
}

a {
  text-decoration: none;
}

.container {
  width: 80%;
  margin: 0 auto;
}

/* header style */
header {
  background-color: #663a82;
  color: #fff;
  padding: 15px 0;
}
.logo {
  a {
    font-weight: bold;
    font-size: 22px;
    line-height: 27px;
    color: #fff;
    span {
      font-weight: 300;
      font-size: 24px;
      line-height: 29px;
      display: block;
    }
  }
}

/* table style */
table {
  margin-top: 20px;
  border-collapse: collapse;
  width: 100%;
  th {
    font-weight: 400;
    font-size: 16px;
    padding: 8px 6px;
    text-align: left;
    background-color: #663a82;
    color: white;
  }
  td {
    font-size: 14px;
  }
  td,
  tr {
    border: 1px solid #ddd;
    padding: 8px;
  }

  tr:nth-child(even) {
    background-color: #f2f2f2;
  }
}
</style>
