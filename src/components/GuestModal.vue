<template>
  <div class="absolute top-0 left-0 w-full h-full flex justify-center bg-black bg-opacity-70 place-items-center backdrop-blur-xl">
    <div class="bg-gray-800 text-white p-9 rounded-lg">
      <h2>Edit Guest</h2>
      <form @submit.prevent="saveEdit">
        <div class="flex flex-col gap-3">
          <label for="editEmail">Tickets:</label>
          <input class="bg-gray-700 border border-gray-300 text-white text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" type="email" id="editEmail" v-model.number="editedGuest.email" required>
        </div>
        <div class="flex flex-col gap-3">
          <label for="editTickets">Tickets:</label>
          <input class="bg-gray-700 border border-gray-300 text-white text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" type="number" id="editTickets" v-model.number="editedGuest.tickets" required>
        </div>
        <div class="flex gap-2 float-right pt-6">
          <button @click="cancelEdit">Cancel</button>
          <button type="submit" class="bg-blue-600 rounded px-4 py-1 hover:bg-blue-700">Save</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    guest: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      editedGuest: { ...this.guest },
    };
  },
  methods: {
    saveEdit() {
      this.$emit('save-edit', { ...this.editedGuest });
    },
    cancelEdit() {
      this.$emit('cancel-edit');
      console.log(this.guest)
    },
  },
};
</script>


