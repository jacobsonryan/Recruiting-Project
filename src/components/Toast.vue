<template>
  <div class="text-white border border-[1px] border-white border-opacity-30 rounded-lg p-12 fixed bottom-2 right-2">
    <p>{{ this.email }} has been added</p>
  </div>
</template>

<script>
export default {
  props: ['email']
}
</script>
