<template>
  <b-container class="mt-5">
    <div v-if="!loading">
      <div>
        <b-button class="mb-5" variant="primary" @click="send">送信</b-button>
      </div>
      <b-form-group label="To:">
        <b-form-input
          type="email"
          placeholder="宛先"
          v-model="mail"
          required
        ></b-form-input>
      </b-form-group>
      <b-form-group label="件名:">
        <b-form-input
          type="text"
          placeholder="件名"
          v-model="title"
          required
        ></b-form-input>
      </b-form-group>
      <b-form-group label="本文:">
        <b-form-textarea
          id="textarea"
          v-model="body"
          placeholder="Enter something..."
          rows="40"
          max-rows="40"
        ></b-form-textarea>
      </b-form-group>
      <b-form-group label="実験ID（触らないでください）:">
        <b-form-input
          type="text"
          placeholder="ID"
          v-model="id"
          required
        ></b-form-input>
      </b-form-group>
    </div>
    <div v-if="loading" class=".mx-auto">
      <b-spinner label="Loading..."></b-spinner>
    </div>
  </b-container>
</template>

<script>
import {
  getFirestore,
  addDoc,
  serverTimestamp,
  collection,
} from "firebase/firestore";

export default {
  data() {
    return {
      body: "",
      title: "",
      mail: "",
      id: "",
      loading: false
    };
  },
  methods: {
    async send() {
      this.loading = true
      const col = collection(getFirestore(), `activities/${this.id}/messages`);
      const result = await addDoc(col, {
        datetime: serverTimestamp(),
        title: this.title,
        mail: this.mail,
        body: this.body,
        id: this.id,
      });
      this.title = ""
      this.mail = ""
      this.body = ""
      this.loading = false
    },
  },
};
</script>
