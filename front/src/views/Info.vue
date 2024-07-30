<template>
    <div>
      <h1>Info Page</h1>
      <p>Item ID: {{ id }}</p>
      <p>{{ apiData.title }}</p>
      <p>{{ apiData.dueBy }}</p>
      <p>{{ apiData.isComplete }}</p>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Info',
    props: ['id'],
    
    computed: {
      id() {
        return this.$route.params.id
      }
    },
    data() {
      return {
        apiData: {}
      }
    },
    methods: {
      async fetchData() {
        try {
          const response = await fetch(`http://localhost:5201/todos/${this.id}`)
          if (!response.ok) {
            throw new Error('Network response was not ok')
          }
          this.apiData = await response.json()
        } catch (error) {
          console.error('There was a problem with the fetch operation:', error)
        }
      }
    },
    created() {
      this.fetchData()
    }
  }
  </script>