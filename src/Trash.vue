<template>
  <div class="inbox-body">
    <app-messages :messages="removedMessages"></app-messages>
  </div>
</template>

<script>
    import Messages from './Messages.vue';

    export default {
      props: {
        data: {
          type: Object,
          required: true
        }
      },
      computed: {
        removedMessages() {
          return this.data.messages.filter(function (message) {
            return ( message.isDeleted === true );
          })
        }
      },
      components: {
        appMessages: Messages
      }
    }
</script>

<style scoped>

</style>
