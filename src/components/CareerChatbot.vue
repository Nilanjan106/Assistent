<template>
  <div class="chatbot-container">
    <div class="chatbot-header">
      <h2 class="futurewise-title">Career Guidance Assistant</h2>
      <div class="chatbot-status" :class="{ online: isOnline }">
        {{ isOnline ? 'Online' : 'Offline' }}
      </div>
    </div>

    <div class="chat-messages" ref="messagesContainer">
      <div v-for="(message, index) in messages" :key="index" 
           :class="['message', message.type]">
        <div class="message-content">
          <div v-if="message.type === 'bot'" class="bot-avatar">
            <img src="@/assets/bot-avatar.png" alt="Bot Avatar" />
          </div>
          <div class="message-text">
            <p>{{ message.text }}</p>
            <div v-if="message.options && message.options.length" class="message-options">
              <button v-for="(option, optIndex) in message.options" 
                      :key="optIndex"
                      @click="selectOption(option)"
                      class="option-button">
                {{ option }}
              </button>
            </div>
          </div>
        </div>
        <div class="message-time">{{ message.time }}</div>
      </div>
    </div>

    <div class="chat-input">
      <input type="text" 
             v-model="userInput" 
             @keyup.enter="sendMessage"
             placeholder="Type your message here..."
             :disabled="!isOnline" />
      <button @click="sendMessage" 
              :disabled="!isOnline || !userInput.trim()"
              class="send-button">
        Send
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CareerChatbot',
  data() {
    return {
      isOnline: true,
      userInput: '',
      messages: [
        {
          type: 'bot',
          text: 'Hello! I\'m your career guidance assistant. How can I help you today?',
          time: this.getCurrentTime(),
          options: [
            'Career Recommendations',
            'Skill Analysis',
            'Learning Resources',
            'Interview Preparation'
          ]
        }
      ]
    }
  },
  methods: {
    getCurrentTime() {
      const now = new Date();
      return now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
    },
    async sendMessage() {
      if (!this.userInput.trim() || !this.isOnline) return;

      // Add user message
      this.messages.push({
        type: 'user',
        text: this.userInput,
        time: this.getCurrentTime()
      });

      const userMessage = this.userInput;
      this.userInput = '';

      // Simulate bot response
      await this.simulateBotResponse(userMessage);

      // Scroll to bottom
      this.$nextTick(() => {
        this.scrollToBottom();
      });
    },
    async simulateBotResponse(userMessage) {
      // Simulate typing delay
      await new Promise(resolve => setTimeout(resolve, 1000));

      // Simple response logic - in a real app, this would call an API
      let response = {
        type: 'bot',
        time: this.getCurrentTime(),
        options: []
      };

      if (userMessage.toLowerCase().includes('career') || userMessage.toLowerCase().includes('job')) {
        response.text = 'I can help you explore career options based on your skills and interests. Would you like to:';
        response.options = ['Take a Career Assessment', 'View Career Matches', 'Explore Industries'];
      } else if (userMessage.toLowerCase().includes('skill') || userMessage.toLowerCase().includes('learn')) {
        response.text = 'Let\'s analyze your skills and identify areas for growth. Would you like to:';
        response.options = ['Take a Skills Assessment', 'View Skill Gaps', 'Get Learning Recommendations'];
      } else {
        response.text = 'I\'m here to help with your career journey. You can ask me about:';
        response.options = ['Career Paths', 'Skills Development', 'Job Search Tips', 'Interview Preparation'];
      }

      this.messages.push(response);
    },
    selectOption(option) {
      this.userInput = option;
      this.sendMessage();
    },
    scrollToBottom() {
      const container = this.$refs.messagesContainer;
      container.scrollTop = container.scrollHeight;
    }
  },
  mounted() {
    this.scrollToBottom();
  }
}
</script>

<style scoped>
.chatbot-container {
  display: flex;
  flex-direction: column;
  height: 600px;
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.chatbot-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background: #3498db;
  color: white;
}

.chatbot-status {
  padding: 0.25rem 0.75rem;
  border-radius: 15px;
  font-size: 0.9rem;
  background: #ff6b6b;
}

.chatbot-status.online {
  background: #2ecc71;
}

.chat-messages {
  flex: 1;
  overflow-y: auto;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.message {
  max-width: 80%;
}

.message.user {
  align-self: flex-end;
}

.message.bot {
  align-self: flex-start;
}

.message-content {
  display: flex;
  gap: 0.5rem;
  align-items: flex-start;
}

.bot-avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  overflow: hidden;
}

.bot-avatar img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.message-text {
  background: #f0f0f0;
  padding: 0.75rem 1rem;
  border-radius: 12px;
  color: #2c3e50;
}

.user .message-text {
  background: #3498db;
  color: white;
}

.message-time {
  font-size: 0.75rem;
  color: #666;
  margin-top: 0.25rem;
  text-align: right;
}

.message-options {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-top: 0.5rem;
}

.option-button {
  background: white;
  border: 1px solid #3498db;
  color: #3498db;
  padding: 0.5rem 1rem;
  border-radius: 20px;
  font-size: 0.9rem;
  cursor: pointer;
  transition: all 0.3s;
}

.option-button:hover {
  background: #3498db;
  color: white;
}

.chat-input {
  display: flex;
  gap: 0.5rem;
  padding: 1rem;
  background: #f5f7fa;
  border-top: 1px solid #eee;
}

.chat-input input {
  flex: 1;
  padding: 0.75rem;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 1rem;
}

.chat-input input:disabled {
  background: #eee;
  cursor: not-allowed;
}

.send-button {
  background: #3498db;
  color: white;
  border: none;
  padding: 0.75rem 1.5rem;
  border-radius: 6px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s;
}

.send-button:disabled {
  background: #ccc;
  cursor: not-allowed;
}

.send-button:hover:not(:disabled) {
  background: #2980b9;
}
</style> 