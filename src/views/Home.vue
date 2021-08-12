<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <SingleProject 
          :project="project" 
          @delete="handleDelete" 
          @complete="handleComplete"
        />
      </div>
    </div>
  </div>
</template>

<script>
import { onMounted, ref } from '@vue/runtime-core'
import SingleProject from '../components/SingleProject.vue'


export default {
  name: 'Home',
  components: {
    SingleProject
  },
  setup(){
    const projects = ref([])
    const error = ref(null)

    onMounted(() => {
      try{
        async function getData(){
          const response = await fetch('http://localhost:3000/projects')
          projects.value = await response.json()
        }
        getData()
      }
      catch(err){
        error.value = err.message
        console.log(error.value);
      }
      console.log(projects.value);
    })

    function handleDelete(id){
      projects.value = projects.value.filter((project) => {
        return project.id !== id
      })
    }

    function handleComplete(id){
      let p = projects.value.find(project => {
        return project.id === id
      })
      p.complete = !p.complete
    }

    return{
      projects,
      handleDelete,
      handleComplete
    }
  }
}
</script>
