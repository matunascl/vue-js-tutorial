<template>
  <div>
    <section v-for="task in tasks" v-bind:key="task.id">
        <Task v-bind:task="task"></Task>
    </section>
  </div>
</template>

<script>
import Task from "./Task"

export default {
  name: 'Tasks',
  props: {
      tasks: Array
  },
  components: {
      Task
  }
}
</script>
  
<style>

</style>