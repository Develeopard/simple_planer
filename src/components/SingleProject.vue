<template>
  <div class="project" :class="{ complete: project.complete}">
      <div class="actions">
          <h3 @click="showDetails = !showDetails">{{ project.title }}</h3>
            <div class="icons">
                <span class="material-icons">edit</span>
                <span @click="deleteProject" class="material-icons">delete</span>
                <span @click="toggleComplete" class="material-icons tick">done</span>
            </div>
      </div>
      <div class="details" v-if="showDetails">
          <p>{{ project.details }}</p>
      </div>
  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
export default {
    props: ['project'],
    setup(props, context){
        const showDetails = ref(false)
        const uri = ref('http://localhost:3000/projects/' + props.project.id)

        const deleteProject = () => {
            fetch(uri.value, { method: 'DELETE' })
                .then(() => context.emit('delete', props.project.id))
                .catch(err => console.log(err))
        }

        const toggleComplete = () => {
            fetch(uri.value, { 
                method: 'PATCH',
                headers: {'Content-Type': 'application/json'},
                body: JSON.stringify({ complete: !props.project.complete})
            }).then(() => {
                context.emit('complete', props.project.id)
            }).catch((err) => console.log(err))
        }

        return{
            showDetails,
            deleteProject,
            uri,
            toggleComplete
        }
    }
}
</script>

<style>
    .project{
        margin: 20px auto;
        background: white;
        padding: 10px 20px;
        border-radius: 4px;
        box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.5);
        border-left: 4px solid rgb(204, 50, 109);
    }

    h3{
        cursor: pointer;
    }

    .actions{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .material-icons{
        font-size: 24px;
        margin-left: 10px;
        color: #bbb;
        cursor: pointer;
    }

    .material-icons:hover{
        color: #777;
    }

    .project.complete{
        border-left: 4px solid lightgreen;
    }

    .project.complete .tick{
        color: lightgreen;
    }
</style>