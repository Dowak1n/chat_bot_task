<template>
    <div class="chat-user-icon" >
        <svg xmlns="http://www.w3.org/2000/svg">
        <path fill="#6e48aa" d="M12,4A4,4 0 0,1 16,8A4,4 0 0,1 12,12A4,4 0 0,1 8,8A4,4 0 0,1 12,4M12,14C16.42,14 20,15.79 20,18V20H4V18C4,15.79 7.58,14 12,14Z" />
        </svg>
    </div>
</template>

<script>
export default {
  data() {
    return {};
  }
}
</script>

<style>
  .chat-user-icon {
    border-radius: 12px 12px 12px 0;
    background: #f3f2f2;
    padding: 6px;
    width: 24px;
    height: 24px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
    margin-left: 8px;
  }
</style>