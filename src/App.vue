<template>
  <div class="app">
    <Calendar :events="events"/>
  </div>
</template>

<script>
import Calendar from "@/components/Calendar"
export default {
	name: 'App',
	components: {Calendar},
	setup(){
		let events = [
			{id:0, name:"18:00 Семинар на какую-то тему", type:"red", date:"01-09-2021"},
			{id:1, name:"18:00 Семинар на какую-то тему", type:"green", date:"01-09-2021"},
			{id:2, name:"18:00 Семинар на какую-то тему", type:"orange", date:"01-09-2021"},
			{id:3, name:"18:00 Семинар на какую-то тему", type:"orange", date:"07-09-2021"},
			{id:4, name:"18:00 Семинар на какую-то тему", type:"red", date:"17-09-2021"},
			{id:5, name:"18:00 Семинар на какую-то тему", type:"green", date:"17-09-2021"},
		]
		return {events}
	}
}
</script>

<style>
</style>
