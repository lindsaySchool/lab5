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
            console.log(data.data);
            // Stel allMessages.data in op een array van objecten met eigenschappen text en user
            allMessages.data = data.data.message.map(message => ({ text: message.text, user: message.user }));
        } catch (error) {
            console.log(error);
        }
    };

    const sendMessage = async () => {
        try {
            const response = await fetch('https://lab5-messages-api-with-mongodb.onrender.com/api/v1/messages', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({ text: message.value, user: 'you' }),
            });
            const data = await response.json();
            allMessages.data.unshift({ text: data.data.message.text, user: data.data.message.user });
            message.value = "";
        } catch (error) {
            console.log(error);
        }
    };
    onMounted(fetchMessages);
</script>

<template>
  <div>
    <ul class="comment-section">
        <li v-for="message in allMessages.data" :key="message._id" class="comment">
            <span>{{ message.user }}</span>
            {{ message.text }}</li>
    </ul>
  </div>
  <div>
        <input v-model="message" type="text" placeholder="Type your message here">
        <button @click="sendMessage">Send</button>
   </div>
</template>

<style scoped>
  .comment-section {
    background-color: #f0f0f0;
    border-radius: 8px;
    padding: 20px;
  }

  .comment {
    background-color: #fff;
    border-radius: 8px;
    margin-bottom: 16px;
    padding: 16px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    list-style: none;
  }

  .comment span {
    font-weight: bold;
    margin-right: 8px;
  }

  input {
    padding: 8px;
    margin: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
  }

  button {
    background: #007bff;
    color: white;
    border-radius: 4px;
    padding: 8px 12px;
    border: none;
    margin: 8px;
    cursor: pointer;
  }
</style>
