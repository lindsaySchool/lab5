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
            allMessages.data = data.data.message;
            console.log(allMessages.data);
        } catch (error) {
            console.log(error);
        }
    };
    console.log(allMessages.data);

    const sendMessage = async () => {
        try {
            const response = await fetch('https://lab5-messages-api-with-mongodb.onrender.com/api/v1/messages', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({ text: message.value }),
            });
            const data = await response.json();
            allMessages.data.push(data.data.message.text);
            message.value = "";
        } catch (error) {
            console.log(error);
        }
    };

    onMounted(fetchMessages);
</script>

<template>
  <div>
    <ul>
        <li v-for="message in allMessages.data" :key="message._id">{{ message }}</li>
    </ul>
  </div>
  <div>
        <input v-model="message" type="text" placeholder="Type your message here">
        <button @click="sendMessage">Send</button>
   </div>
</template>

<style scoped>

</style>
