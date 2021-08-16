<template>
  <form @submit.prevent="handleSubmit">
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
import { useRouter } from 'vue-router'
export default {
    props: ['id'],
    setup(props){
        const title = ref('')
        const details = ref('')

        const router = useRouter()

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

        function handleSubmit(){
            fetch(uri.value, {
                method: 'PATCH',
                headers: {'Content-Type': 'application/json'},
                body: JSON.stringify({title: title.value, details: details.value})
            }).then(() => {
                router.push('/')
            }).catch((err) => console.log(err))
        }
        
        return{
            title,
            details,
            uri,
            handleSubmit
        }
    }
}
</script>

<style>

</style>