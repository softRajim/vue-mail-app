<template>
  <div class="inbox-body">
    <div class="inbox-option">
      <button class="btn btn-primary" @click="goBack">
        <i class="fa fa-arrow-left"></i>&nbsp; Back
      </button>
      <button class="btn btn-danger" @click="data.message.isDeleted = true" :disabled="data.message.isDeleted">
        <i class="fa fa-remove"></i> &nbsp; Delete
      </button>
      <template v-if="typeof data.message.isRead != 'undefined'">
        <button class="btn btn-primary" @click="data.message.isRead = false" :disabled="!data.message.isRead">
          <i class="fa fa-envelope-open" aria-hidden="true"></i>&nbsp; Mark As Unread
        </button>
        <button class="btn btn-primary" @click="data.message.isRead = true" :disabled="data.message.isRead">
          <i class="fa fa-envelope"></i>&nbsp; Mark as read
        </button>
      </template>
    </div>

    <p><strong>Date:</strong>{{data.message.date.fromNow()}}</p>
    <p><strong>From:</strong> {{data.message.from.name}} :: {{data.message.from.email}}</p>
    <hr>
    <div v-html="data.message.content" class="message"></div>
    <div v-if="data.message.attachments.length > 0" class="attachment">
      <h4>Attachments</h4>
      <ul>
        <li v-for="attachment in data.message.attachments">
          <i class="fa fa-paperclip"></i> {{ attachment.fileName }} ({{ attachment.size | formatBytes }})
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  import { eventBus } from './main.js';
  export default {
    props: {
      data: {
        type: Object,
        required: true
      }
    },
    activated() {
      if (typeof this.data.message.isRead != 'undefined') {
        this.data.message.isRead = true
      }
    },
    methods: {
      goBack() {
        let previousView = this.$parent.previousView;
        eventBus.$emit('changeView', {
          tag: previousView.tag,
          title: previousView.title,
          data: previousView.data
        });
      }
    },
    filters: {
      formatBytes(num) {
        if (typeof num !== 'number' || isNaN(num)) {
          throw new TypeError('Expected a number');
        }
        var exponent;
        var unit;
        var neg = num < 0;
        var units = ['B', 'kB', 'MB', 'GB', 'TB', 'PB', 'EB', 'ZB', 'YB'];
        if (neg) {
          num = -num;
        }
        if (num < 1) {
          return (neg ? '-' : '') + num + ' B';
        }
        exponent = Math.min(Math.floor(Math.log(num) / Math.log(1000)), units.length - 1);
        num = (num / Math.pow(1000, exponent)).toFixed(2) * 1;
        unit = units[exponent];
        return (neg ? '-' : '') + num + ' ' + unit;
      }
    }
  }
</script>

<style scoped>

</style>
