<template>
  <div>
    <p @click.prevent="enableEditing" v-show="!editing">{{ <!--conversation.data.text--> }}</p>
    <v-text-field v-model="messageText"
                  v-show="editing"
                  @keydown.enter="disableEditing"
                  @keydown.esc="disableEditing"
                  @keydown.tab="disableEditing"
    ></v-text-field>
  </div>
</template>

<script>
export default {
  name: "Message",
  props: {
    conversation: {
      type: Object
    }
  },
  async mounted() {
   /* this.messageText = this.props.conversation.data.text;
    this.editing = this.props.conversation.editing;*/
    console.log(this);
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
      const conversation = Object.assign({}, this.props.conversation);
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