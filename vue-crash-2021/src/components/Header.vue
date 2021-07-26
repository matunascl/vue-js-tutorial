<template>
    <header>
        <h1>{{ title }}</h1>
        <Button 
        @btn-click="$emit('toggle-add-task')" 
        v-bind:text="showAddTask ? 'Close' : 'Add Task'" 
        v-bind:color="showAddTask ? 'red' : 'green'"/>
    </header>
</template>

<script>
import Button from '@/components/Button'

export default {
    name: 'Header',
    props: {
        title: String,
        showAddTask: Boolean
    },
    components: {
        Button
    }
}
</script>

<style scoped>
  header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
  }

</style>