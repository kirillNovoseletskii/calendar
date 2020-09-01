<template>
  <div id="app">
    <h1>Test Task(calendar)</h1>
      <calendar :year='2020' :month='2' :day='23'/>
  </div>
</template>
<script>
import calendar from './components/calendar' 
export default {
  name: 'App',
  components: {
    calendar
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
