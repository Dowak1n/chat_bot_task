<template>
  <div class="chat-position" :class="{ 'show': isChatVisible }">
    <div class="chat-window">
      <ChatText
      v-for="(scene, key) in dialogues"
      :key="key"
      :bot-question="scene.text"
      :user-responses="scene.choices"
      @select-response="selectResponse"
    ></ChatText>
    </div>
  </div>
</template>

<script>
import ChatText from './ChatText.vue';

export default {
  data() {
    return {
      dialogues: {
        'start': {
          text: "Привет! какую пиццу ты хочешь?",
          choices: [
            { text: "Хочу пеперони!", nextScene: 'pepperoni' },
            { text: "Хочу пиццу c ветчиной.", nextScene: 'ham' },
          ]
        }
      },

      dialogVariables: {
        'pepperoni': {
          text: 'Какой размер вам нужен?',
          choices: [
            { text: 'Большой', nextScene: 'good' },
            { text: 'Средний', nextScene: 'bad' }
          ]
        },
        'ham': {
          text: 'Любите ветчину?',
          choices: [
            { text: 'Да, очень!', nextScene: 'good' },
            { text: 'Нет, не очень', nextScene: 'bad' }
          ]
        },
        'good': {
          text: 'Хорошо',
          choices: [
            { text: 'Да, очень!', nextScene: 'next' },
            { text: 'Нет, не очень', nextScene: 'next' }
          ]
        },
        'bad': {
          text: 'Плохо',
        },
        'next': {
          text: 'Ну, как скажешь',
        },
      },
      currentStep: 'start',
      isChatVisible: false,
    };
  },

  methods: {
    selectResponse(clickedIndex, currentScene) {
      this.dialogues[this.currentStep].choices = this.dialogues[this.currentStep].choices.splice(clickedIndex, 1);
      this.currentStep = currentScene;

      if (this.dialogVariables[currentScene]) {
        const variables = this.dialogVariables[currentScene];
        this.dialogues[currentScene] = variables;
        this.dialogVariables[currentScene] = '';
      }
    },
  },

  mounted() {
    setTimeout(() => {
      this.isChatVisible = true;
    }, 1000);
  },
  components: { ChatText },
};
</script>

<style>
.chat-position {
  position: fixed;
  bottom: 0;
  right: 0;
  padding: 10px;
  z-index: 999;
  opacity: 0;
  transition: opacity 0.5s ease-in-out, transform 0.5s ease-in-out;
  transform: translateY(100%);
}

.chat-window {
  display: flex;
  flex-direction: column;
  max-width: 240px;
  overflow-y: scroll;
  background: #f5f8fb;
  border-radius: 12px 12px 0 12px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  padding: 12px;
  height: 320px;
}

.chat-window::-webkit-scrollbar {
  width: 0;
  height: 0;
}

.chat-position.show {
  opacity: 1;
  transform: translateY(0);
}
</style>