<template>
<h1>Vue front</h1>
<form :action="sendMessage" @click.prevent="onSubmit">
  <input v-model="message" type="text">
  <input type="submit" value="Send" @click="sendMessage">
</form>
<!--
<p>
{{ message }}
</p> -->
<div v-if="showMsg">  <!-- OVDE NESTO DIV NE PRIKAZUJE KAD IMA VIF -->
  <h3>Message  in WebSocket</h3>
<p>
{{ rcvMessage }}
</p>
<button @click="showMsg = !showMsg">Drop</button> 
</div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      message: "",
      socket: null,
      rcvMessage: "",
      showMsg: false
    }
  },
  mounted() {
  this.socket = new WebSocket("ws://localhost:9100/socket")
  this.socket.onmessage = (msg) => {
    this.acceptMsg(msg)
  }
},
methods: {
  sendMessage() {
    let msg = {
      "greeting": this.message
    }
    this.socket.send(JSON.stringify(msg))
  },
  acceptMsg(msg) {
    this.rcvMessage = msg.data
    this.showMsg = true
  }
}
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #267c5f;
  margin-top: 60px;
}
</style>
