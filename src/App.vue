<script>
import ModalWindow from './components/ModalWindow.vue'

export default {
  components: {
    ModalWindow
  },
  data: function() {
    return {
      open: false
    }
  }
}
</script>

<template>
  <div class="flex w-100 justify-center">
    <button @click="open = true" class="h-12 w-[160px] bg-gradient rounded-md px-3 py-3 text-base text-white">
      Invite Users
    </button>
  </div>
  <ModalWindow v-model:openWindow="open"></ModalWindow>
</template>

