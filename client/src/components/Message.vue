<template>
  <div>
    <p @click.prevent="enableEditing()" v-show="!editing">{{ message.data.text }}</p>
    <v-text-field v-model="messageText"
                  v-show="editing"
                  label="message.data.text"
                  @keydown.enter="disableEditing()"
                  @keydown.esc="disableEditing()"
                  @keydown.tab="disableEditing()"
    ></v-text-field>
  </div>
</template>

<script>
export default {
  name: "Message",
  props: {
    message: {
      type: Object
    }
  },
  async mounted() {
    this.messageText = this.props.message.data.text;
    this.editing = this.props.message.editing;
  },
  data() {
    return {
      messageText: '',
      editing: false,
    }
  },
  methods: {
    disableEditing() {
      this.editing = false;
      const conversation = Object.assign({}, this.props.message);
      conversation.editing = true;
      conversation.message.data.text = this.messageText;
      this.$emit("disable-editing", conversation);
    },
    enableEditing() {
      this.editing = true;
    }
  }
}
</script>

<style scoped>

</style>