<script setup>
    //import ref, reactive and onMounted from 'vue';
    import { ref, reactive, onMounted } from 'vue';
    //ref -> iets reflecteren naar de frontend
    let message =ref("");//int, string, boolean
    let allMessages = reactive({
        data: [],
    });//array, object

    const fetchMessages = async () => {
        try {
            const response = await fetch('https://lab5-messages-api-with-mongodb.onrender.com/api/v1/messages');
            const data = await response.json();
            console.log(data)
            allMessages.data = data.data.message;
        } catch (error) {
            console.log(error);
        }
    };

    onMounted(fetchMessages);
</script>

<template>
  <div>
    <ul>
        <li v-for="m in allMessages.data" :key="m._id">{{ m }}</li>
    </ul>
  </div>
  <div>
        <input v-model="message" type="text" placeholder="Type your message here">
        <button @click="sendMessage">Send</button>
   </div>
</template>

<style scoped>

</style>
