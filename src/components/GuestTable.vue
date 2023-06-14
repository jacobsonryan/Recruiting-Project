<template>
  <div>
    <GuestForm @add-guest="addGuest" />
    <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400 border border-opacity-40 border-white">
      <thead class="text-xs text-gray-400 uppercase bg-black dark:bg-gray-700 dark:text-gray-400 tracking-wider border-b border-opacity-20 border-white ">
        <tr>
          <th scope="col" class="px-6 py-3">Email</th>
          <th scope="col" class="px-6 py-3">Tickets ({{ totalTickets }})</th>
          <th scope="col" class="px-6 py-3">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(guest, index) in guests" :key="index" class="border-b border-opacity-20 border-white text-gray-100">
          <td class="px-6 py-4">{{ guest.email }}</td>
          <td class="px-6 py-4">{{ guest.tickets }}</td>
          <td class="px-6 py-4 flex gap-3">
            <div @click="editGuest(index)" class="cursor-pointer">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                <path stroke-linecap="round" stroke-linejoin="round" d="M16.862 4.487l1.687-1.688a1.875 1.875 0 112.652 2.652L10.582 16.07a4.5 4.5 0 01-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 011.13-1.897l8.932-8.931zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0115.75 21H5.25A2.25 2.25 0 013 18.75V8.25A2.25 2.25 0 015.25 6H10" />
              </svg>
            </div>
            <div @click="deleteGuest(index)" class="cursor-pointer">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                <path stroke-linecap="round" stroke-linejoin="round" d="M9.75 9.75l4.5 4.5m0-4.5l-4.5 4.5M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    <div v-show="showModal"  class="absolute top-0 left-0 w-full h-full flex justify-center bg-black bg-opacity-70 place-items-center backdrop-blur-xl">
      <div class="bg-black border border-[1px] border-white border-opacity-50 text-white p-9 rounded-xl">
        <h2>Edit Guest</h2>
        <div class="flex flex-col gap-3">
          <label for="editEmail">Tickets:</label>
          <input class="bg-black border border-white border-opacity-50 outline-none text-white text-sm rounded-lg w-full p-2.5" type="email" id="editEmail" v-model.number="editedGuest.email" required>
        </div>
        <div class="flex flex-col gap-3">
          <label for="editTickets">Tickets:</label>
          <input class="bg-black border border-white border-opacity-50 outline-none text-white text-sm rounded-lg w-full p-2.5" type="number" id="editTickets" v-model.number="editedGuest.tickets" required>
        </div>
        <div class="flex gap-2 float-right pt-6">
          <button @click="cancelEdit" class="text-xs">Cancel</button>
          <button @click="saveEdit" class="w-min bg-white text-black text-xs rounded-lg px-5 py-2.5">Save</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import GuestForm from './GuestForm.vue';
import GuestRepository from '../guest-repository.js';

export default {
  components: {
    GuestForm,
  },
  data() {
    return {
      guests: [],
      editingIndex: null,
      maxTickets: 20,
      showModal: false,
      editedGuest: { email: '', tickets: 0 },
    };
  },
computed: {
    totalTickets() {
      let total = 0;
      this.guests.forEach((guest, index) => {
        if (index !== this.editingIndex) {
          total += guest.tickets;
        }
      });
      return total;
    },
  },
  methods: {

    loadGuests() {
      const repository = new GuestRepository();
      repository.load()
        .then((data) => {
          this.guests = data;
          data.forEach(guest => {
            this.newTotalTickets += guest.tickets
          })
        })
        .catch((error) => {
          console.error('Error loading guests:', error);
        });
    },

    addGuest(guest) {
      const totalTickets = this.guests.reduce((total, guest) => total + guest.tickets, 0);
      const newTotalTickets = totalTickets + guest.tickets;

      if (newTotalTickets > this.maxTickets) {
        alert(`The maximum number of tickets (${this.maxTickets}) has been reached.`);
        return; 
      }

      this.guests.push(guest);

      const repository = new GuestRepository();
      repository.save(this.guests)
        .catch((error) => {
          console.error('Error saving guests:', error);
        });
    },

    editGuest(index) {
      this.editedGuest = { ...this.guests[index] };
      this.showModal = true;
      this.editingIndex = index;
    },

    saveEdit() {
      const editedGuestIndex = this.editingIndex;
      const editedGuest = { ...this.editedGuest };
      const totalTickets = this.totalTickets + editedGuest.tickets;

      if (totalTickets > this.maxTickets) {
        alert(`The maximum number of tickets (${this.maxTickets}) has been reached.`);
        return;
      }

      this.guests.splice(editedGuestIndex, 1, editedGuest);
      this.editingIndex = null;
      this.editedGuest = { email: '', tickets: 0 };

      this.showModal = false;

      const repository = new GuestRepository();
      repository.save(this.guests)
        .catch((error) => {
          console.error('Error saving guests:', error);
        });
    },

    cancelEdit() {
      this.editingIndex = null;
      this.showModal = false
    },

    deleteGuest(index) {
      this.guests.splice(index, 1);

      const repository = new GuestRepository();
      repository.save(this.guests)
        .catch((error) => {
          console.error('Error saving guests:', error);
        });
    },
  },

  created() {
    this.loadGuests();
  },
};
</script>

