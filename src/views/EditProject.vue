<template>
  <form>
      <label>Title:</label>
      <input type="text" v-model="title" required>
      <label>Details:</label>
      <textarea v-model="details" required></textarea>
      <button>Update Project</button>
  </form>
</template>

<script>
import { ref } from '@vue/reactivity'
import { onMounted } from '@vue/runtime-core'
export default {
    props: ['id'],
    setup(props){
        const title = ref('')
        const details = ref('')

        const uri = ref('http://localhost:3000/projects/' + props.id)

        onMounted(() => {
            fetch(uri.value)
                .then(res => res.json())
                .then(data => {
                    // console.log(data);
                    title.value = data.title
                    details.value = data.details
                })
                console.log('component mounted');
        })
        
        return{
            title,
            details,
            uri
        }
    }
}
</script>

<style>

</style>