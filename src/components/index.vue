<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

    <table style="text-align: center;">
      <tr>
        <th>USD - United States Dollars <br> <b>USD</b></th>
        <th>10000</th>
      </tr>
      <tr>
        <td>Jill</td>
        <td>50</td>
      </tr>
      <tr>
        <td>Eve</td>
        <td>94</td>
      </tr>
    </table>
  </div>
</template>

<script>
  import Vuesax from 'vuesax';

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
