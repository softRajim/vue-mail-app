<template>
  <table v-if="messages" class="table table-inbox table-hover">
    <tbody>
      <tr v-for=" message in messages"
          @click="viewMessageDetail(message)"
          :class=" { unread: typeof message.isRead !== 'undefined' && !message.isRead}">
        <td><input type="checkbox"></td>
        <td>
          <a href="#" v-if="typeof message.isImportant !== 'undefined'"
             @click="message.isImportant = !message.isImportant" >
            <i :class="['fa', 'fa-star', {important: message.isImportant}]"></i>
          </a>
        </td>
        <td>{{ message.from.name }}</td>
        <td>{{ message.subject }}</td>
        <td><i v-if="message.attachment" class="fa fa-paperclip"></i></td>
        <td>{{ message.data }}</td>
      </tr>
    </tbody>
  </table>
  <p v-else >No messages here yet.</p>
</template>

<script>
  import {eventBus} from "./main";

  export default {
      props: {
        messages: {
          type: Array,
          required: true
        }
      },
      methods: {
        viewMessageDetail(message) {
          eventBus.$emit('changeView', {
            tag: 'app-view-message',
            title: message.title,
            data: {
              message: message
            }
          });
        }
      }
    }
</script>

<style scoped>

</style>
