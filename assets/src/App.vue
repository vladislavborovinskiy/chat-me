<script setup lang="ts">
import {Header, WelcomeScreen, QuizScreen, FormScreen} from './components';
import {ref, computed, onMounted, onUnmounted} from 'vue';

interface Question {
  question: string;
  options: string[];
}

interface WelcomeMessage {
  id: number;
  text: string;
}

const welcomeMessages = ref<WelcomeMessage[]>([
  {
    id: 1,
    text: 'Hello, my name is Sarah K! I am your assistant and personal manager on the Offer name platform.',
  },
  {
    id: 2,
    text: 'Now you have the opportunity to take advantage of promotions from Canadian and foreign companies and earn from $335 in the first weeks!',
  },
  {id: 3, text: 'Please answer the following questions so I can help you and get started:'},
]);

const originalQuestions: Question[] = [
  {question: 'Are you a resident of Canada?', options: ['Yes', 'No']},
  {question: 'Are you a migrant?', options: ['Yes', 'No']},
  {question: 'How old are you?', options: ['18-24', '40-55', '25-40', '55+']},
  {question: 'Do you have children under 18?', options: ['Yes', 'No']},
  {
    question:
      'Great! One more important question - do you have a Canadian bank card to receive dividends?',
    options: ['Yes, I have a Canadian bank card', 'No, only a foreign card'],
  },
  {
    question:
      'Offer name uses artificial intelligence with a forecast accuracy of 93.7%. How much time per day are you willing to spend on earning?',
    options: ['Maximum 30 minutes', '1-2 hours'],
  },
  {
    question:
      'Offer name works fully automatically and requires only 5 minutes a day! What monthly income would suit you?',
    options: ['$5,000 - $10,000', '$20,000 - $25,000', '$10,000 - $20,000', 'More than $25,000'],
  },
  {
    question:
      'Wonderful. Registration will take only 2 minutes. Below are our advantages, what is most important for you to get?',
    options: [
      'Personal financial consultant 24/7',
      'Platform that provides 93.7% successful deals',
      'Ability to withdraw funds at any time',
      'Full protection of your investments',
    ],
  },
];

const questions = ref<Question[]>([...originalQuestions]);

const reorderQuestionOptions = () => {
  const isMobile = window.innerWidth <= 1024;

  const tempQuestions = [...originalQuestions];

  if (isMobile) {
    tempQuestions[2].options = ['18-24', '25-40', '40-55', '55+'];
    tempQuestions[6].options = [
      '$5,000 - $10,000',
      '$10,000 - $20,000',
      '$20,000 - $25,000',
      'More than $25,000',
    ];
  } else {
    tempQuestions[2].options = ['18-24', '40-55', '25-40', '55+'];
    tempQuestions[6].options = [
      '$5,000 - $10,000',
      '$20,000 - $25,000',
      '$10,000 - $20,000',
      'More than $25,000',
    ];
  }

  questions.value = tempQuestions;
};

onMounted(() => {
  reorderQuestionOptions();
  window.addEventListener('resize', reorderQuestionOptions);
});

onUnmounted(() => {
  window.removeEventListener('resize', reorderQuestionOptions);
});

const currentStage = ref<'welcome' | 'quiz' | 'form'>('welcome');
const currentQuestion = ref(0);
const selectedOption = ref<string | null>(null);

const getCurrentQuestion = computed(() => questions.value[currentQuestion.value]);
const progressPercentage = computed(
  () => ((currentQuestion.value + 1) / questions.value.length) * 100
);

const startQuiz = () => {
  currentStage.value = 'quiz';
};

const SetAnswer = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    setTimeout(() => {
      NextQuestion();
    }, 1000);
  } else {
    setTimeout(() => {
      currentStage.value = 'form';
    }, 1000);
  }
};

const NextQuestion = () => {
  const activeElement = document.activeElement as HTMLElement | null;
  activeElement?.blur();
  currentQuestion.value++;
  selectedOption.value = null;
};
</script>

<template>
  <main class="app">
    <div class="chat">
      <h1 class="chat__logo visible-tablet">Offer name</h1>

      <Header v-if="currentStage !== 'form'" />

      <WelcomeScreen
        v-if="currentStage === 'welcome'"
        :welcomeMessages="welcomeMessages"
        @startQuiz="startQuiz" />

      <QuizScreen
        v-if="currentStage === 'quiz'"
        v-model="selectedOption"
        :currentQuestion="getCurrentQuestion"
        :progressPercentage="progressPercentage"
        @setAnswer="SetAnswer" />

      <FormScreen v-if="currentStage === 'form'" />
    </div>
  </main>
</template>
