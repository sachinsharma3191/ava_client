<template>
  <div class="messages">
    <v-simple-table>
      <template v-slot:default>
        <tbody>
        <tr v-for="(conversation,index) in conversations" :key="index">
          <td @click.prevent="enableEditing(conversation)" v-show="!conversation.editing">
            {{conversation.data.text }}
          </td>
          <td v-show="conversation.editing">
            <v-text-field v-model="conversation.data.text"
                          v-show="conversation.editing"
                          @keydown.enter="disableEditing(conversation)"
                          @keydown.esc="disableEditing(conversation)"
                          @keydown.tab="disableEditing(conversation)"></v-text-field>
          </td>
        </tr>
        </tbody>
      </template>
    </v-simple-table>
    <v-btn @click.prevent="sendMessage">Send</v-btn>
  </div>
</template>

<script>
import axios from 'axios';
import * as util from '../util/Util';

export default {
  name: "Messages",
  async mounted() {
    await axios.get(util.BASE_URL + "/conversations").then(resp => {
      let conversations = [];

      for (let conver of resp.data.conversations) {
        const conversation = {
          ...conver,
          editing: false
        }
        conversations.push(conversation);
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
      conversation.editing = false;
    },
    enableEditing(conversation) {
      conversation.editing = true;
    },
    sendMessage() {
      console.log("Message sent");
    }
  }
}
</script>

<style scoped>
.messages {
  margin-top: 20px  ;
}
</style>