<template>
  <div class="flex justify-center place-items-center my-32 flex-col">
    <form @submit.prevent="addGuest" class="flex flex-col gap-4">
      <div class="flex flex-col gap-3 text-white">
        <label for="email">Email</label>
        <input class="bg-black border border-white border-opacity-50 outline-none text-white text-sm rounded-lg w-full p-2.5" type="email" id="email" v-model="newGuest.email" required>
      </div>
      <div class="flex flex-col gap-3 text-white">
        <label for="tickets">Tickets</label>
        <input class="bg-black border border-white border-opacity-50 outline-none text-white text-sm rounded-lg w-full p-2.5" type="number" id="tickets" v-model.number="newGuest.tickets" required>
      </div>
      <button type="submit" class="w-min bg-white text-black text-xs rounded-lg px-5 py-2.5">Submit</button>
    </form>
    <Toast  :email="email" :class="{ 'fade-in': submitted, 'fade-out': !submitted }"></Toast>
  </div>
</template>

<script>
import Toast from './Toast.vue'

export default {
  components: {
    Toast
  },
  props: ['submitted', 'email'],
  data() {
    return {
      newGuest: {
        email: '',
        tickets: null
      },
    };
  },
  methods: {
    addGuest() {
      this.$emit('add-guest', { ...this.newGuest });
      this.newGuest.email = '';
      this.newGuest.tickets = null;
    },
  },
};
</script>


<style>
.fade-in {
  opacity: 1;
  transition: opacity 0.25s ease-in;
}

.fade-out {
  opacity: 0;
  transition: opacity 0.25s ease-out;
}
</style>
