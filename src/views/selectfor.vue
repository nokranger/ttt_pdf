<template>
  <div>
    <!-- <nav>
      <router-link to="/dashboard" style="font-size: 20px;">Import & Export</router-link> ||
      <router-link to="/Attached" style="font-size: 20px;">Attached</router-link> ||
      <router-link to="/payroll" style="font-size: 20px;">Payroll</router-link>
    </nav> -->
    <b-container>
      <div style="display: inline;">
        <h1 style="text-shadow: 2px 2px 5px black;display: inline;font-size: 25px;">Report</h1>
        <b-form-select
          style="display: inline; margin: 10px;width: 300px;height: 40px;font-family: 'Noto Serif', sans-serif;font-weight: bold;font-size: 20px;border-radius:10px;border:1px solid #cccccc;"
          id="selectoption" v-model="selectOption" :options="optionss"></b-form-select>
      </div>
      <div v-if="selectOption == 1">
        <do01></do01>
      </div>
      <div v-if="selectOption == 2">
        <do02></do02>
      </div>
      <div v-if="selectOption == 3">
        <do03></do03>
      </div>
      <div v-if="selectOption == 4">
        <do04></do04>
      </div>
      <div v-if="selectOption == 5">
        <do05></do05>
      </div>
      <div v-if="selectOption == 6">
        <do06></do06>
      </div>
      <div v-if="selectOption == 7">
        <do07></do07>
      </div>
      <div v-if="selectOption == 8">
        <do08></do08>
      </div>
      <div v-if="selectOption == 9">
        <do09></do09>
      </div>
      <div v-if="selectOption == 10">
        <do10></do10>
      </div>
      <div v-if="selectOption == 11">
        <do11></do11>
      </div>
      <div v-if="selectOption == 12">
        <cb01></cb01>
      </div>
      <div v-if="selectOption == 13">
        <ck01></ck01>
      </div>
    </b-container>
  </div>
</template>

<script>
import do01 from '../views/do01.vue'
import do02 from '../views/do02.vue'
import do03 from '../views/do03.vue'
import do04 from '../views/do04.vue'
import do05 from '../views/do05.vue'
import do06 from '../views/do06.vue'
import do07 from '../views/do07.vue'
import do08 from '../views/do08.vue'
import do09 from '../views/do09.vue'
import do010 from '../views/do10.vue'
import do011 from '../views/do11.vue'
import cb01 from '../views/cb01.vue'
import ck01 from '../views/ck01.vue'

export default {
  components: {
    do01,
    do02,
    do03,
    do04,
    do05,
    do06,
    do07,
    do08,
    do09,
    do010,
    do011,
    cb01,
    ck01
  },
  data() {
    return {
      selectOption: null,
      optionss: [
        { value: null, text: 'เลือก' },
        { value: 1, text: 'do01' },
        { value: 2, text: 'do02' },
        { value: 3, text: 'do03' },
        { value: 4, text: 'do04' },
        { value: 5, text: 'do05' },
        { value: 6, text: 'do06' },
        { value: 7, text: 'do07' },
        { value: 8, text: 'do08' },
        { value: 9, text: 'do09' },
        { value: 10, text: 'do10' },
        { value: 11, text: 'do11' },
        { value: 12, text: 'cb01' },
        { value: 13, text: 'ck01' },
      ],
    }
  }
}
</script>