<template>
  <div v-if="isBotVisible">
    <div class="chat-bot">
      <RobotIcon/>
      <div class="chat-bot-text">{{ botQuestion }}</div>
    </div>

    <div v-if="!disable">
      <div
        v-for="(item, index) in userResponses"
        :key="index"
        @click="openChat(index, item.nextScene)"
        class="chat-user-text"
      >
        {{ item.text }}
      </div>
    </div>
  </div>

  <div v-if="disable" class="chat-user">
    <div v-if="!isTextVisible" class="chat-dot">
      <div
        v-for="index in 3"
        :key="index"
        :style="{ opacity: currentDot === index ? '0.5' : '1' }"
        class="dot"
      ></div>
    </div>
    <div v-if="isTextVisible">
      <div
        class="chat-user-choise"
        v-for="(item, index) in userResponses"
        :key="index"
      >
        {{ item.text }}
      </div>
    </div>
    <AccountIcon/>
  </div>
</template>

<script>
import RobotIcon from '../assets/RobotIcon.vue';
import AccountIcon from '../assets/AccountIcon.vue';

export default {
  data() {
    return {
      disable: false,
      isBotVisible: false,
      isTextVisible: false,
      currentDot: null,
      dots: [0, 1, 2],
    };
  },
  mounted() {
    setInterval(() => {
      if (this.currentDot !== null) {
        this.currentDot = (this.currentDot + 1) % this.dots.length;
      } else {
        this.currentDot = 0;
      }
    }, 200);
    setTimeout(() => {
      this.isBotVisible = true;
    }, 1000);
  },
  props: {
    botQuestion: String,
    userResponses: Array,
  },
  methods: {
    openChat(clickedIndex, currentScene) {
      this.$emit('select-response', clickedIndex ,currentScene);
      this.disable = true
    },
  },
  emits: ['select-response'],
  watch: {
    disable(newValue, oldValue) {
      if (newValue !== oldValue) {
        setTimeout(() => {
          this.isTextVisible = true;
          console.log(this.isTextVisible);
        }, 1000);
      }
    },
  },
  components: { RobotIcon, AccountIcon },
};
</script>

<style>
  .chat-bot {
    align-items: flex-end;
    display: flex;
    justify-content: flex-start;
    margin-bottom: 16px;
  }

  .chat-bot-text {
    display: block;
    padding: 8px;
    color: #ffff;
    background: #6e48aa;
    border-radius: 12px 12px 12px 0;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
    display: inline-block;
    opacity: 0;
    transform: scaleX(0) scaleY(0);
    transform-origin: left bottom;
    animation: stretchIn 0.2s ease-out forwards;
    font-family: Quicksand, sans-serif;
    font-size: 13px;
  }

  .chat-user {
    align-items: flex-end;
    display: flex;
    justify-content: flex-end;
    margin-bottom: 10px;
  }

  .chat-user-choise {
    background: #ffff;
    color: #000000;
    border-radius: 12px 12px 0 12px;
    padding: 8px;
    box-shadow: rgba(0, 0, 0, 0.15) 0px 1px 2px 0px;
    transform: scaleX(0) scaleY(0);
    transform-origin: right bottom;
    animation: stretchIn 0.2s ease-out forwards;
    font-family: Quicksand, sans-serif;
    font-size: 13px;
  }

  .chat-user-text {
    cursor: pointer;
    font-family: Quicksand, sans-serif;
    font-size: 13px;
    display: block;
    padding: 8px;
    color: #ffff;
    background: #6e48aa;
    border-radius: 16px;
    box-shadow: 0 1px 10px rgba(0, 0, 0, 0.1);
    margin-bottom: 10px;
    width: max-content;
    transition: transform 0.3s ease-in-out;
  }

  .chat-user-text:hover {
    transform: scale(1.05);
    background: #7a5daa;
  }

  .chat-dot {
    height: 18px;
    align-items: center;
    display: flex;
    border-radius: 12px 12px 0 12px;
    color: #ffff;
    background: #6e48aa;
    padding: 8px 4px 8px 8px;
    text-align: end;
    transform: scaleX(0) scaleY(0);
    transform-origin: right bottom;
    animation: stretchIn 0.2s ease-out forwards;
  }

  .dot-block {
    display: flex;
    flex-direction: row;
    padding: 10px;
  }

  .dot {
    width: 3px;
    height: 3px;
    border-radius: 3px;
    margin-right: 4px;
    background: #ffff;
  }

  @keyframes stretchIn {
    to {
      opacity: 1;
      transform: scaleX(1) scaleY(1);
    }
  }
</style>