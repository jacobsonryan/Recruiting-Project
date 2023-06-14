<template>
  <div>
    <guest-table></guest-table> 
  </div>
</template>

<script>
  import GuestTable from './GuestTable.vue'
  // eslint-disable-next-line no-unused-vars
  const GuestRepository = require('../guest-repository');

  export default {
    data: () => {
      return { };
    },
    components: {
      GuestTable,
    }
  }
</script>
