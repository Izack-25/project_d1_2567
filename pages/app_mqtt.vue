<template>
  <div class="text-center">
    <v-progress-circular :model-value="value" :rotate="360" :size="100" :width="15" color="green">
      <template v-slot:default> {{ value }} % </template>
    </v-progress-circular>
    &nbsp;
    <v-progress-circular :model-value="value" :rotate="360" :size="100" :width="15" color="red">
      <template v-slot:default> {{ value1 }} % </template>
    </v-progress-circular>
  </div>
  
</template>
<script>
//   const mqtt = require('mqtt')
import mqtt from "mqtt";
definePageMeta({
layout: "iott",
});
export default {
data() {
  return {
    id: 10,
    name: 'hello',
    msg: '',
    sw: '',
    value: 10,
    value1: 5
  }
}, // data

created() {
  this.client = mqtt.connect('ws://broker.emqx.io:8083/mqtt')
  this.client.on('connect', this.onMqttConnect.bind(this))
  this.client.on('message', this.onMqttMessage.bind(this))
}, // created

beforeDestroy() {
  // if (this.client) {
  //   this.client.end()
  // }
  this.client && this.client.end()
}, // beforeDestroy

methods: {
  add() {
    let sw = this.id++
    this.client.publish('op-001', String(sw))
  }, // add

  onMqttConnect() {
    console.log('connected')
    this.client.publish('op', 'status')
    this.client.subscribe('status', err => err)
    this.client.subscribe('room1', err => err)
  }, // onMqttConnect

  onMqttMessage(topic, message) {
    if (topic === 'status') {
      // message is Buffer
      console.log('GOT:', message.toString())
      this.msg = message.toString()
    }
    if(topic === 'room1'){
      console.log('lamp 1 On')
      
      this.value = message
      this.value1 = message
    }
  }, // onMqttMessage
}, // methods
}
</script>