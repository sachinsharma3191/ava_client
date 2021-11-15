<template>
  <div>
    <li v-for="conversation in conversations" :key="conversation.message_id">
      {{conversation}}
      <message :conversation="conversation" @disable-editing="disableEditing"/>
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
      let conversations = [];

      for (let conversation of resp.data.conversations) {
        const message = {
          ...conversation,
          editing: false
        }
        conversations.push(message);
      }
      this.conversations = conversations;
    }).catch(err => {
      this.error = err;
    })
  },
  data() {
    return {
      conversations: [],
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