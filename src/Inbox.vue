<template>
  <div class="inbox-body">
    <div class="btn-group">
      <button class="btn" @click="refresh">
        <i class="fa fa-refresh"></i>
        &nbsp; Refresh</button>
    </div>
    <app-messages :messages="incomingMessages"></app-messages>
  </div>
</template>

<script>
    import Messages from './Messages.vue';
    import { eventBus } from "./main";

    export default {
      props: {
        data: {
          type: Object,
          require: true
        }
      },
      methods: {
        refresh() {
          eventBus.$emit('refreshPage');
        }
      },
      computed: {
        incomingMessages() {
          return this.data.messages.filter(function (message) {
            return (message.type == 'incoming' && !message.isDeleted);
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
