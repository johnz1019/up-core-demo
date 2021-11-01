<template>
  <div>
    <b>Hello world</b>
    <div v-if="username">
      {{ username }}
      <button value="logout" @click="logout">logout</button>
    </div>
    <div v-else>
      <button value="login" @click="connect">login</button>
    </div>
    <br />
    <div>
      <textarea v-model="message"></textarea>
      <div><button value="authorize" @click="authorize">authorize</button></div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import UP from "up-core";
import { UPAuthMessage } from "./up-types";

export default Vue.extend({
  data() {
    return {
      username: "",
      message: "TO BE SIGNED MESSAGE",
    };
  },
  mounted() {
    UP.config(
      "http://localhost:8000",
      "http://localhost:8000/connect",
      "http://localhost:8000/authorize"
    );
  },
  methods: {
    async connect() {
      console.log("connect clicked");
      try {
        const account = await UP.connect();
        this.username = account.username;
        console.log("account", account);
      } catch (err) {
        console.log("connect err", err);
      }
    },
    logout() {
      console.log("connect clicked");
      UP.disconnect();
      this.username = "";
    },
    async authorize() {
      console.log("authorize clicked");

      try {
        const resp = await UP.authorize(
          new UPAuthMessage("PLAIN_MSG", this.username, this.message)
        );
        console.log("resp", resp);
      } catch (err) {
        console.log("auth err", err);
      }
    },
  },
});
</script>
