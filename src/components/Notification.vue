<template>
  <transition
    enter-active-class="animated fadeInDown"
    leave-active-class="animated fadeOutRight"
  >
    <div v-show="show" class="bubble">
      <div class="message">{{ text }}</div>
    </div>
  </transition>
</template>

<script>
export default {
  name: "Notification",
  data() {
    return {
      show: false
    };
  },
  mounted() {
    this.show = true;
    setTimeout(() => {
      // this show(notification) will be hidden after 3sec
      this.show = false;
    }, 3000);
  },
  props: {
    text: ""
  }
};
</script>
<style scoped lang="scss">
.bubble {
  border-radius: 6px;
  position: absolute;
  top: 10%;
  padding: 5px 10px;
  background-color: $blue;
}

.message {
  @include medium-text;
  color: $white;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
