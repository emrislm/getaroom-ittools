<template>
  <v-row no-gutters align="center" justify="center">
    <v-col cols="auto">
      <v-card min-width="290" color="#001e26">
        <Snackbar v-model="snackbar" :text="message" />

        <v-card-title>
          <h2>Login</h2>
        </v-card-title>
        <v-card-text>
          <v-form  ref="form" v-model="isValid" lazy-validation @submit.prevent="submit">
            <v-text-field v-model="user.name" background-color="#001e26" :counter="16" :rules="nameRules" label="Your name:" outlined required />
            <v-text-field v-model="user.room" background-color="#001e26" :counter="16" :rules="roomRules" label="To room:" outlined required />

            <v-btn :disabled="!isValid" color="#00DC82" text outlined class="mt-3" type="submit">Submit</v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import { mapActions } from "vuex";
import Snackbar from "@/components/Snackbar";
import messageDict from "@/lib/messageDict";

export default {
  name: "Home",
  layout: "login",
  components: {
    Snackbar,
  },
  data: () => ({
    isValid: true,
    user: {
      name: "",
      room: "",
      typingStatus: false,
    },
    nameRules: [
      v => !!v || "Name is required",
      v => (v && v.length <= 16) || "Name must be less than 16 characters",
    ],
    roomRules: [
      v => !!v || "Enter the room",
      v => (v && v.length <= 16) || "Room must be less than 16 characters",
    ],
    snackbar: false,
  }),
  computed: {
    message() {
      const { message } = this.$route.query;
      return messageDict[message] || "";
    },
  },
  mounted() {
    this.snackbar = !!this.message;
  },

  methods: {
    ...mapActions(["createUser"]),
    submit() {
      if (this.$refs.form.validate()) {
        this.createUser(this.user);
        this.$router.push("/chat");
      }
    },
  },

  head: {
    title: "Getaroom - ittools",
  },
};
</script>

<style lang="scss">

</style>
