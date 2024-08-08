<template>
  <div id="app">
    <!-- <nav>
      <router-link to="/dashboard">Import & Export</router-link> || 
      <router-link to="/Attached">Attached</router-link> ||
      <router-link to="/payroll">Payroll</router-link>
    </nav> -->
    <nav>
      <!-- <b-form-select
      style="display: inline; margin: 10px;width: 300px;height: 40px;font-family: 'Noto Serif', sans-serif;font-weight: bold;font-size: 20px;border-radius:10px;border:1px solid #cccccc;"
      id="selectoption" v-model="selectOption" :options="optionss" onchange=""></b-form-select> -->
      <!-- <router-link to="/dashboard" style="font-size: 20px;">Import & Export</router-link> ||
      <router-link to="/Attached" style="font-size: 20px;">Report</router-link> ||
      <router-link to="/pattached" style="font-size: 20px;">Previous Report</router-link> ||
      <router-link to="/payroll" style="font-size: 20px;">Payroll</router-link> ||
      <router-link to="/searchtrip" style="font-size: 20px;">Search Trip</router-link> ||
      <router-link to="/updatepayment" style="font-size: 20px;">Update Status Allowance & OT</router-link> ||
      <router-link to="/exportpayment" style="font-size: 20px;">Export Payment</router-link> -->
    </nav>
    <router-view/>
  </div>
</template>
<script>
export default {
  components: {
  },
  data () {
    return {
      selectOption: null,
      optionss: [
        { value: null, text: 'เลือก' },
        { value: 'http://github.com', text: 'do01' },
        { value: 8, text: 'do02' },
        { value: 81, text: 'do03' },
        { value: 9, text: 'do04' },
        { value: 10, text: 'do05' },
        { value: 11, text: 'do06' },
        { value: 12, text: 'do07' },
        { value: 12, text: 'do08' },
        { value: 12, text: 'do09' },
        { value: 12, text: 'do10' },
        { value: 12, text: 'do11' },
      ],
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Noto+Serif');
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: #dbdbdb;
  height: 1500px;
  font-family: 'Noto Serif', sans-serif;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
