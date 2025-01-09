<script setup lang="ts">
import { ref, onMounted, nextTick } from 'vue';

const props = defineProps({
  welcomeMessages: {
    type: Array as () => Array<{ id: number; text: string }>,
    required: true,
  },
});

const emit = defineEmits(['startQuiz']);

const visibleMessages = ref<Array<{ id: number; text: string }>>([]);
const currentIndex = ref(0);
const allMessagesShown = ref(false);
let timeoutId: ReturnType<typeof setTimeout> | null = null;

const chatContainer = ref<HTMLUListElement | null>(null);

const scrollToBottom = () => {
  if (chatContainer.value) {
    chatContainer.value.scrollTop = chatContainer.value.scrollHeight;
  }
};

const emitRedirect = () => {
  emit('startQuiz');
};

const startAutoRedirect = () => {
  timeoutId = setTimeout(() => {
    emitRedirect();
  }, 8000);
};

const handleButtonClick = () => {
  if (timeoutId) clearTimeout(timeoutId);
  emitRedirect();
};

onMounted(() => {
  const displayMessages = () => {
    if (currentIndex.value < props.welcomeMessages.length) {
      visibleMessages.value.push(props.welcomeMessages[currentIndex.value]);
      currentIndex.value++;
      nextTick(() => {
        scrollToBottom();
      });
      if (currentIndex.value === props.welcomeMessages.length) {
        allMessagesShown.value = true;
        startAutoRedirect();
      } else {
        setTimeout(displayMessages, 2000);
      }
    }
  };

  displayMessages();
});
</script>

<template>
  <section class="body-welcome">
    <ul class="body-welcome__messages" ref="chatContainer">
      <li 
        v-for="message in visibleMessages" 
        :key="message.id" 
        class="body-welcome__messages-text"
      >
        {{ message.text }}
      </li>
    </ul>
    <button 
      v-if="allMessagesShown" 
      type="button" 
      class="button" 
      @click="handleButtonClick"
    >
      Join now
    </button>
  </section>
</template>
