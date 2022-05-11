<template>
    <div id="component">
        <div class="arrow-buttons">
            <button>
                <font-awesome-icon v-on:click="toggleOnOff()" icon="power-off" />
            </button>
            <button id='BACK' v-on:click="sendButton($event)">
                <font-awesome-icon icon="angle-left" />
            </button>
        </div>
        <div class="arrow-buttons">
            <div class="button-row">
                <button id='UP' v-on:click="sendButton($event)">
                    <font-awesome-icon icon="arrow-up" />
                </button>
            </div>
            <div class="button-row">
                <button id='LEFT' v-on:click="sendButton($event)">
                    <font-awesome-icon icon="arrow-left" />
                </button>
                <button id='ENTER' v-on:click="sendButton($event)">
                    <font-awesome-icon icon="square" />
                </button>
                <button id='RIGHT' v-on:click="sendButton($event)">
                    <font-awesome-icon icon="arrow-right" />
                </button>
            </div>
            <div class="button-row">
                <button id='DOWN' v-on:click="sendButton($event)">
                    <font-awesome-icon icon="arrow-down" />
                </button>
            </div>
            <div class="button-row">
                <button id='HOME' v-on:click="sendButton($event)">
                    <font-awesome-icon icon="bars" />
                </button>
            </div>
        </div>
        <input v-model="volume" class="custom-slider" min="1" max="15" step="1"
        @change="setVolume" type="range" name="volume" id="volume">
        <p>{{ this.volume }}</p>
    </div>
</template>

<script>

import axios from '@/services/axios';

export default {
    data() {
        return {
            volume: 1
        }
    },
    mounted() {
        window.addEventListener('keyup', this.keyHandler)

        // Get volume from TV
        axios.get('/command?command=audio/getVolume').then((response) => {
            this.volume = response.data.volume;
        });
    },
    methods: {
        keyHandler: function(event) {
            switch(event.keyCode) {
                case 8:
                    this.sendButton('BACK')
                    break;
                case 13:
                    this.sendButton('ENTER')
                    break;
                case 32:
                    this.sendButton('HOME')
                    break;
                case 37:
                    this.sendButton('LEFT')
                    break;
                case 38:
                    this.sendButton('UP')
                    break;
                case 39:
                    this.sendButton('RIGHT')
                    break;
                case 40:
                    this.sendButton('DOWN')
                    break;
                case 187:
                    this.increaseVolume()
                    break;
                case 189:
                    this.decreaseVolume()
                    break;
            }
        },
        toggleOnOff: function() {
            axios.get(`/button/toggleOnOff`).then(() => {
                console.debug(`Comando enviado!`);
            });
        },
        increaseVolume: function() {
            this.volume++;

            let payload = {
                'command': 'audio/setVolume',
                'payload': {
                    'volume': Number(this.volume)
                }
            };

            axios.post('/command', payload).then((res) => {
                console.debug(res.data);
            })
        },
        decreaseVolume: function() {
            this.volume--;

            let payload = {
                'command': 'audio/setVolume',
                'payload': {
                    'volume': Number(this.volume)
                }
            };

            axios.post('/command', payload).then((res) => {
                console.debug(res.data);
            })
        },
        setVolume: function(event) {
            const newVolumeValue = event.target.value;

            let payload = {
                'command': 'audio/setVolume',
                'payload': {
                    'volume': Number(newVolumeValue)
                }
            };

            axios.post('/command', payload).then((res) => {
                console.debug(res.data);
            })
        },
        sendButton: function(event) {
            let button = event?.currentTarget?.id;

            if(!button) {
                button = event;
            }

            axios.get(`/button?keyName=${button}`).then(() => {
                console.debug(`Botão ${button} enviado!`);
            });
        }
    }
}
</script>

<style scoped>
#component {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 360px;
    margin-top: 15px;
}

* {
    background: #000;
    color: #fff;
}

p {
    margin: 2px;
    font-weight: 800;
    font-size: 42px;
}

button {
    outline: none;
    border: none;
    padding: 12px;
    margin: 0 2px;
    border-radius: 28px;
    background: #545454;
    transition: 0.4s;
    font-style: normal;
    font-weight: 700;
    font-size: 12px;
}

button:active {
    background: #155260;
}

svg[class*="fa"] {
    --icon-size: 52px;
    --margin-side: 5px;
    width: var(--icon-size);
    height: var(--icon-size);
    margin: 0px var(--margin-side);
    background: transparent;
    color: #ffffff;
}

.custom-slider {
    margin-top: 15px;
}

.button-row {
    margin-top: 4px;
}
</style>