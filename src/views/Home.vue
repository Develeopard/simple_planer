<template>
  <div class="home">
    <FilterNav @filterChange="current = $event"  :current="current"/>
    <div v-if="projects.length">
      <div v-for="project in filteredProjects()" :key="project.id">
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
import FilterNav from '../components/FilterNav.vue'


export default {
  name: 'Home',
  components: {
    SingleProject,
    FilterNav
  },
  setup(){
    const projects = ref([])
    const error = ref(null)
    const current = ref('all')

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
      // console.log(projects.value);
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

    function filteredProjects(){
      if( current.value === 'completed'){
        return projects.value.filter(project => project.complete)
      }
      if( current.value === 'ongoing'){
        return projects.value.filter(project => !project.complete)
      }
      return projects.value
    }

    return{
      projects,
      handleDelete,
      handleComplete,
      current,
      filteredProjects
    }
  }
}
</script>
