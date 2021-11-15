<template>
  <div>
    <li v-for="conversation in messages" :key="conversation.messageId">
      <message :message="conversation" @disable-editing="disableEditing"/>
    </li>
    <v-btn @click.prevent="sendMessage">Send</v-btn>
  </div>
</template>

<script>
import axios from 'axios';
import * as util from '../util/Util';
import Message from "./Message";

export default {
  name: "Messages",
  components: {
    Message
  },
  async mounted() {
    await axios.get(util.BASE_URL + "/conversations").then(resp => {
      console.log(resp);
      const response = resp.data.conversations;
      let messages = [];

      for (let conversation of response.data.conversations) {
        const message = {
          ...conversation,
          editing: false
        }
        messages.push(message);
      }
      this.messages = messages;
    }).catch(err => {
      this.error = err;
    })
  },
  data() {
    return {
      messages: [],
      error: null,
    }
  },
  methods: {
    disableEditing(conversation) {
      console.log(conversation);
    },
    sendMessage() {
      console.log("Message sent");
    }
  }
}
</script>

<style scoped>

</style>