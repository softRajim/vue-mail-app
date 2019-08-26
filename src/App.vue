<template>
  <div class="container">
    <div class="mail-box">
      <app-sidebar :messages="messages"></app-sidebar>
      <app-content :messages="messages"></app-content>
    </div>
  </div>
</template>

<script>
  import Sidebar from './Sidebar.vue';
  import Content from './Content.vue';
  import messages from './data/messages.js';
  import { eventBus} from "./main";

  export default {
    data() {
      return {
        messages: messages
      }
    },
    created() {
      eventBus.$on('sentMessage', (data) => {
        let temp = [data.message];
        this.message = temp.concat(this.message.splice(0));
      });
    },
    components: {
      appSidebar: Sidebar,
      appContent: Content
    }
  }
</script>
