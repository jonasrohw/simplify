<style scoped>
.chatbox::-webkit-scrollbar {
  width: 0;
  height: 0;
}


.chatbox {
  margin-top: 2vh;
  padding: 10px;
  height: 55vh;
  overflow-y: auto;
  border-radius: 0.5vw;
  scrollbar-width: none; /* For Firefox */
  -ms-overflow-style: none;  /* For Internet Explorer and Edge */
  scroll-behavior: smooth;

}
.chat-interface {
  display: flex;
  align-items: stretch;
}

.chat-input {
  padding: calc((1vh + 1vw) / 2);
  margin-left: 2vw;
  width: 60%;
  flex-grow: 1;
  border: 1px solid #e0e0e0;
  border-radius: 2em;
  background-color: #FFF;
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 2vh;
}

.send-button {
  margin-left: 1vw;
  margin-right: 2vw;
  width: calc((5vw + 5vh) / 2);
  height: calc((5vw + 5vh) / 2);
  border: none;
  color: white;
  border-radius: 50%;
  cursor: pointer;
  font-size: 1em;
  transition: transform 0.2s;
  background-color: #FFCA28;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
}

.send-button:hover {
  transform: scale(1.1);
  background-color: #FFD700;
}
.message {
  margin: 10px;
  padding: 8px;
  border-radius: 10px;
  animation: fadeIn 0.6s;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);

}
.user-message {
  background: linear-gradient(to top right, #FFCA28, #FFD700);
  color: #000000;
  border-radius: 18px 18px 0 18px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 12px 18px;
  margin-bottom: 10px;
  align-self: flex-end;
  font-size: 16px;
}
.received-image {
  max-width: 100%;
  height: auto;
  border-radius: 5px;
}

.bot-message {
  background: linear-gradient(to top right, #ffffff, #e0e0e0);
  color: #000000;
  border-radius: 18px 18px 18px 0;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 12px 18px;
  margin-bottom: 10px;
  font-size: 16px;
}

.list-enter-active,
.list-leave-active {
  transition: opacity 0.6s;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.chat-wrapper {

  width: 100%;
  height: 100%;
  position: relative;

}

</style>

<template>
	<main >
    <div class="bg-amber-400 h-[1px] my-[2.5vh]"></div>
    <section class="mt-0 mb-[1vh]">
      <Stories />
    </section>
    <div class="bg-amber-400 h-[1px] my-[2.5vh]"></div>
    <BalanceCard />
	</main>
  <section>
    <div class="chat-wrapper">
      <div class="chatbox" ref="chatbox">
        <TransitionGroup name="list" tag="div">
          <div v-for="msg in messages" :key="msg.id" class="message" :class="msg.type">
            <div v-if="msg.text">{{ msg.text }}</div>
            <img v-if="msg.image" :src="msg.image" alt="Received Image" class="received-image">
            <LineChartComponent v-if="msg.type === 'line-chart'" :data="msg.data" />
          </div>
        </TransitionGroup>
      </div>
      <div class="chat-interface">
        <input v-model="userInput" class="chat-input" type="text" placeholder="Send a message" @keyup.enter="sendMessage" />
        <button class="send-button" @click="sendMessage"><i class="fas fa-arrow-up"></i></button>
      </div>
    </div>
  </section>
</template>	


<script setup>

import { ref, onMounted, nextTick, watch } from 'vue';
import HeaderHome from '@/components/HeaderHome.vue'
import BalanceCard from '@/components/BalanceCard.vue'
import axios from 'axios';
import Stories from "@/components/Stories.vue";
import ChartComponent from "@/components/ChartComponent.vue";
import LineChartComponent from '@/components/LineChartComponent.vue';

const isMac = navigator.platform.toUpperCase().indexOf('MAC') >= 0;
const baseURL = import.meta.env.VUE_APP_API_BASE_URL || `http://127.0.0.1:${isMac ? '5001' : '5000'}`;
console.log(baseURL);
const userInput = ref("");
const messages = ref([]);
const chatbox = ref(null);

async function sendMessage() {
  if (userInput.value.trim() !== "") {
    const userMessage = {
      id: Date.now(),
      type: "user-message",
      text: userInput.value,
      image: null,
    };

    messages.value.push(userMessage);

    const lowerCaseText = userInput.value.toLowerCase();
    userInput.value = "";

    // Special frontend handlers
    if (lowerCaseText === "show me a graph" || lowerCaseText === "show me a line chart" || lowerCaseText === "show me an image") {
      // ...
      if (lowerCaseText === "show me a graph") {
      messages.value.push({
        id: Date.now() + 1,
        type: "bar-chart",  // Assuming you have a bar chart component
        data: {
          labels: ['Red', 'Blue', 'Yellow', 'Green', 'Purple', 'Orange'],
          datasets: [{
            label: '# of Votes',
            data: [12, 19, 3, 5, 2, 3],
            backgroundColor: [
              'rgba(255, 99, 132, 0.2)',
              'rgba(54, 162, 235, 0.2)',
              'rgba(255, 206, 86, 0.2)',
              'rgba(75, 192, 192, 0.2)',
              'rgba(153, 102, 255, 0.2)',
              'rgba(255, 159, 64, 0.2)'
            ],
            borderColor: [
              'rgba(255, 99, 132, 1)',
              'rgba(54, 162, 235, 1)',
              'rgba(255, 206, 86, 1)',
              'rgba(75, 192, 192, 1)',
              'rgba(153, 102, 255, 1)',
              'rgba(255, 159, 64, 1)'
            ],
            borderWidth: 1
          }]
        }
      });
    } 
    else if (lowerCaseText === "show me a line chart") {
  messages.value.push({
    id: Date.now() + 1,
    type: "line-chart",
    data: {
      labels: ['January', 'February', 'March', 'April', 'May'],
      datasets: [
        {
          label: 'Monthly Expenses',
          data: [-15, -25, -35, -20, -30],
          fill: false,
          borderColor: 'rgb(255, 99, 132)',
          tension: 0.1
        },
        {
          label: 'Monthly Profits',
          data: [10, 20, 30, 40, 50],
          fill: false,
          borderColor: 'rgb(75, 192, 192)',
          tension: 0.1
        },
      ]
    },
    options: {
      scales: {
        y: {
          beginAtZero: false,
        },
      },
      responsive: true,
      maintainAspectRatio: false,
    },
  });
}
 
    } else {
      try {
        // Send the user message to the backend
        const response = await axios.post(`${baseURL}/message`, { userMessage: userMessage.text });
        const apiResponse = response.data;
        
        // Extract the bot reply from the response
        const botReply = apiResponse.botReply;
        
        // Handle different types of bot replies
        if (botReply.type === "message") {
          messages.value.push({
            id: Date.now() + 1,
            type: "bot-message",
            text: botReply.content,
            image: null
          });
        } else if (botReply.type === "line-chart") {
          messages.value.push({
          id: Date.now() + 1,
          type: "line-chart",
          data: botReply.content.data,
          options: botReply.content.options
        });
      } 
 
       else if (botReply.type === "image") {
          messages.value.push({
            id: Date.now() + 1,
            type: "bot-message",
            text: "",
            image: botReply.content
          });
        } 
      } catch (error) {
        console.error("Error connecting to the server:", error);
        messages.value.push({
          id: Date.now() + 1,
          type: "bot-message",
          text: "Unable to connect to the server.",
          image: null
        });
      }
    }
  }
  scrollToBottom();
}
const scrollToBottom = () => {
  nextTick(() => {
    if (chatbox.value) {
      chatbox.value.scrollTop = chatbox.value.scrollHeight;
    }
  });
};

watch(userInput, () => {
  scrollToBottom();
});

onMounted(async () => {
  try {
    await axios.delete(`${baseURL}/delete-messages`);
  } catch (error) {
    console.error("Failed to delete messages:", error);
  }

  messages.value.push({
    id: Date.now(),
    type: "bot-message",
    text: "Hello John! How can I help you today?"
  });

  scrollToBottom();
});

</script>