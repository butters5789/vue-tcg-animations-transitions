<template>
  <div class="container">
    <div class="block" :class="{ animate: animateBlock }"></div>
    <button @click="startAnimateBlock">Animate</button>
  </div>

  <div class="container">
    <transition
      :css="false"
      @before-enter="beforeEnterParaTransition"
      @enter="enterParaTransition"
      @after-enter="afterEnterParaTransition"
      @before-leave="beforeLeaveParaTransition"
      @leave="leaveParaTransition"
      @after-leave="afterLeaveParaTransition"
      @enter-cancelled="enterCancelledParaTransition"
      @leave-cancelled="leaveCancelledParaTransition"
    >
      <p v-if="paraIsVisible">This is only sometimes visible...</p>
    </transition>
    <button @click="toggleParaVisibility">Toggle Paragraph</button>
  </div>

  <div class="container">
    <transition name="fade-button" mode="out-in">
      <button v-if="!usersAreVisible" @click="showUsers">Show Users</button>
      <button v-else @click="hideUsers">Hide Users</button>
    </transition>
  </div>

  <base-modal @close="hideDialog" :open="dialogIsVisible">
    <p>This is a test dialog!</p>
    <button @click="hideDialog">Close it!</button>
  </base-modal>

  <div class="container">
    <button @click="showDialog">Show Dialog</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      animateBlock: false,
      dialogIsVisible: false,
      paraIsVisible: false,
      usersAreVisible: false,
      enterInterval: null,
      leaveInterval: null,
    };
  },
  methods: {
    enterCancelledParaTransition() {
      clearInterval(this.enterInterval);
    },
    leaveCancelledParaTransition() {
      clearInterval(this.leaveInterval);
    },
    beforeEnterParaTransition(el) {
      console.log('Before enter para transition');
      console.log(el);

      el.style.opacity = 0;
    },
    enterParaTransition(el, done) {
      console.log('Enter para transition');
      console.log(el);

      let round = 1;
      this.enterInterval = setInterval(() => {
        el.style.opacity = round * 0.01;
        round = round + 1;
        if (round > 100) {
          clearInterval(this.enterInterval);
          done();
        }
      }, 20);
    },
    afterEnterParaTransition(el) {
      console.log('After enter para transition');
      console.log(el);
    },
    beforeLeaveParaTransition(el) {
      console.log('Before leave para transition');
      console.log(el);

      el.style.opacity = 1;
    },
    leaveParaTransition(el, done) {
      console.log('Leave para transition');
      console.log(el);

      let round = 1;
      this.leaveInterval = setInterval(() => {
        el.style.opacity = 1 - round * 0.01;
        round = round + 1;
        if (round > 100) {
          clearInterval(this.leaveInterval);
          done();
        }
      }, 20);
    },
    afterLeaveParaTransition(el) {
      console.log('After leave para transition');
      console.log(el);
    },
    startAnimateBlock() {
      this.animateBlock = true;
    },
    showDialog() {
      this.dialogIsVisible = true;
    },
    showUsers() {
      this.usersAreVisible = true;
    },
    hideUsers() {
      this.usersAreVisible = false;
    },
    toggleParaVisibility() {
      this.paraIsVisible = !this.paraIsVisible;
    },
    hideDialog() {
      this.dialogIsVisible = false;
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
button {
  font: inherit;
  padding: 0.5rem 2rem;
  border: 1px solid #810032;
  border-radius: 30px;
  background-color: #810032;
  color: white;
  cursor: pointer;
}
button:hover,
button:active {
  background-color: #a80b48;
  border-color: #a80b48;
}
.block {
  width: 8rem;
  height: 8rem;
  background-color: #290033;
  margin-bottom: 2rem;
}
.container {
  max-width: 40rem;
  margin: 2rem auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 2rem;
  border: 2px solid #ccc;
  border-radius: 12px;
}

.animate {
  animation: slide-fade 0.3s ease-out forwards;
}

.fade-button-enter-from,
.fade-button-leave-to {
  opacity: 0;
}

.fade-button-enter-active {
  transition: opacity 0.3s ease-out;
}

.fade-button-leave-active {
  transition: opacity 0.3s ease-in;
}

.fade-button-enter-to,
.fade-button-leave-from {
  opacity: 1;
}

@keyframes slide-fade {
  0% {
    transform: translateX(0) scale(1);
  }

  70% {
    transform: translateX(-120px) scale(1.1);
  }

  100% {
    transform: translateX(-150px) scale(1);
  }
}
</style>
