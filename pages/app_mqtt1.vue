<template>
    <div class="container">
        <v-row class="text-center">
            <!-- Temperature Card -->
            <v-col cols="12">
                <v-card class="mx-auto" max-width="1000" hover>
                    <v-card-title>
                        <v-col cols="12">
                            <h2>Temperature</h2>
                        </v-col>
                    </v-card-title>
                    <v-card-text>

                        <v-row>
                            <v-col cols="6">
                                <v-progress-circular :model-value="value" :rotate="360" :size="204" :width="34"
                                    color="blue">
                                    <template v-slot:default> {{ value }} % </template>
                                </v-progress-circular>
                            </v-col>

                            <v-col cols="6">
                                <v-progress-circular :model-value="value1" :rotate="360" :size="204" :width="34"
                                    color="red">
                                    <template v-slot:default> {{ value1 }} % </template>
                                </v-progress-circular>
                            </v-col>
                        </v-row>
                    </v-card-text>
                </v-card>
            </v-col>

            <!-- Control Card -->
            <v-col cols="12">
                <v-card class="mx-auto" max-width="1000" hover>
                    <v-card-text>
                        <v-row>
                            <v-col cols="6">
                                <v-card-title>CONTROL</v-card-title>
                                <v-btn block style="background-color: greenyellow;color: #fff;"
                                    @click="start">Start</v-btn>
                                <br>
                                <v-btn block style="background-color: red;color: #fff;" @click="stop">Stop</v-btn>
                                <br>
                            </v-col>

                            <v-col cols="6">
                                <v-row class="d-flex align-center justify-center">
                                    <v-col cols="6">
                                        <br><br>
                                        <div class="rounded-circle mx-auto"
                                            :style="{ height: '32px', width: '32px', background: isOn ? 'green' : '#EEE' }">
                                        </div>
                                        ON
                                    </v-col>
                                    <v-col cols="6">
                                        <br><br>
                                        <div class="rounded-circle mx-auto"
                                            :style="{ height: '32px', width: '32px', background: !isOn ? 'red' : '#EEE' }">
                                        </div>
                                        OFF
                                    </v-col>
                                </v-row>
                                <v-progress-linear color="light-blue" height="10" :model-value="value"
                                    striped></v-progress-linear>
                                <v-progress-linear color="deep-orange" height="10" :model-value="value1"
                                    striped></v-progress-linear>
                                ผลรวม
                                <v-progress-linear color="lime" height="10" :model-value="totalValue"
                                    striped></v-progress-linear>
                            </v-col>
                        </v-row>
                    </v-card-text>
                </v-card>
            </v-col>
        </v-row>

        <br>

        <!-- Light Bulb Card -->
        <div class="container">
            <v-row>
                <!-- Control Panel on the Left -->
                <v-col cols="4">
                    <v-card class="mx-auto" max-width="300" hover>
                        <v-card-title>
                            <h3>Light Control</h3>
                        </v-card-title>
                        <v-card-text>
                            <v-row>
                                <!-- Buttons for controlling lights -->
                                <v-col v-for="index in 6" :key="index" cols="12">
                                    <v-btn @click="controlLights(index)" class="mt-2" block
                                        :style="{ backgroundColor: getButtonColor(index) }"
                                        :disabled="!isOn">
                                        {{ getButtonLabel(index) }}
                                    </v-btn>
                                </v-col>
                            </v-row>
                        </v-card-text>
                    </v-card>
                </v-col>

                <!-- Light Bulbs on the Right -->
                <v-col cols="8">
                    <v-card class="mx-auto" max-width="700" hover>
                        <v-card-text>
                            <v-row class="text-center">
                                <!-- Displaying 3 light bulbs -->
                                <v-col v-for="(isOnlight, index) in lightStates.slice(0, 3)" :key="index" cols="4">
                                    <div class="bulb-wrapper">
                                        <div class="bulb" :style="{ backgroundColor: isOnlight ? 'yellow' : '#ddd' }">
                                        </div>
                                        <div class="bulb-base"></div>
                                    </div>
                                </v-col>
                            </v-row>
                        </v-card-text>
                    </v-card>
                </v-col>
            </v-row>
        </div>
    </div>
</template>
<script>
import mqtt from "mqtt";
definePageMeta({
    layout: "iott",
});
export default {
    data() {
        return {
            value: 10,
            value1: 5,
            originalValue: 10,
            originalValue1: 5,
            isOn: false,
            isOnlight: false,
            lightStates: [false, false, false] // Array to hold the state of each light
        };
    },

    computed: {
        totalValue() {
            return this.value + this.value1;
        }
    },

    created() {
        this.client = mqtt.connect('ws://broker.emqx.io:8083/mqtt');
        this.client.on('connect', this.onMqttConnect.bind(this));
        this.client.on('message', this.onMqttMessage.bind(this));
    },

    beforeDestroy() {
        this.client && this.client.end();
    },

    methods: {
        start() {
            this.isOn = true;
            this.value = this.originalValue;
            this.value1 = this.originalValue1;
        },
        stop() {
            this.isOn = false;
            this.value = 0;
            this.value1 = 0;
            this.lightStates = [false, false, false]; // Turn off all lights
        },
        onMqttConnect() {
            this.client.publish('op', 'status');
            this.client.subscribe('status');
            this.client.subscribe('room1');
        },
        onMqttMessage(topic, message) {
            if (topic === 'status') {
                this.msg = message.toString();
            }
            if (topic === 'room1' && this.isOn) {
                this.value = Number(message.toString());
                this.value1 = Number(message.toString());
            }
        },
        controlLights(buttonIndex) {
            // เพื่อไม่ให้เปิดใช้งานเมื่อยังไม่ได้เปิดตัวควบคุม
            if (!this.isOn) {
                alert('โปรดเปิดการทำงานของแผ่งควบคุมก่อน!');
                return;
            }
            // Update lightStates based on the button index
            switch (buttonIndex) {
                case 1:
                    this.lightStates = [true, false, false];
                    break;
                case 2:
                    this.lightStates = [false, true, false];
                    break;
                case 3:
                    this.lightStates = [false, false, true];
                    break;
                case 4:
                    this.lightStates = [true, true, false];
                    break;
                case 5:
                    this.lightStates = [true, true, true];
                    break;
                default:
                    this.lightStates = [false, false, false];
                    break;
            }
        },
        getButtonColor(index) {
            // Returns button color based on index
            return this.lightStates[index - 1] ? 'lightgray' : '#ffeb3b';
        },
        getButtonLabel(index) {
            // Returns button label based on index
            if (index === 6) return 'Turn All Off';
            if (index === 5) return 'Turn All On';
            if (index === 4) return 'Turn Lights 1 & 2 On';
            if (index === 3) return 'Turn Light 3 On';
            if (index === 2) return 'Turn Light 2 On';
            if (index === 1) return 'Turn Light 1 On';
            return '';
        },
        
    },
};
</script>
<style scoped>
.bulb-wrapper {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.bulb {
    width: 100px;
    height: 150px;
    border-radius: 50% 50% 0 0;
    background-color: #ddd;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}

.bulb-base {
    width: 80px;
    height: 30px;
    background-color: #888;
    border-radius: 0 0 15px 15px;
    margin-top: -10px;
}
</style>
