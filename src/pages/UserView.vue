<template>
    <div>
         <base-marquee :text='marqueeText' svg-id='touchme'></base-marquee>
         <transition name='welcome-transition'>
             <div class='welcome' v-if='!audioIsActive'>
                <h3 lang="en" class='call-to-connect'><span class='title'>EVER HEAR THISBODY?</span> <br><br><span class='cta'>pwease touch bewow nyaaaaa</span></h3>
                <marquee-button class='yes-button' @click='startAudio' text='UWU' clicktext='OWO' />
            </div>
            <h3
                v-else
                lang="en"
                class='now-touching'
                @pointerdown='testTone'
                >Ů̹̟̣̲̥͇͎͚̲̟̅̓̂̂̓͋̂̓͐̿͐́̕̕͟W̸̡̡̬̱̤̬̬̩̫͙̻̠͕͕̣͖͔̾ͭ͑̃̾̀ͦ̏̌͊̚͘U̡͗ͮ̄ͫ͛̂̆̈ͦͫ̈́ͣ̚̚̚͘͝͏͓͕̹̥̮̳̬̬̙̟͈͕͇̗͕͘W̢̡̛̫̞̪̞̲͓̤̲͍̻̦̥̮̮̹͆̓̓̾̎ͨ̊͝Ų̤̥̳͔͍̫ͬ͗̀͑̑́̚͜Ẉ̷̶̨̛̫̖̱̪̬͓͚̜̣̮̙̫̮̬̹̎̀ͯ̓̂ͪͯ͆̇͐ͯ̂ͯ̌̐̋̐ͣ̚ͅÙ̷̘̯͓͔̞͙̟͍͕̠̗̺̫͔͙̮̆͌ͬ̍̑̇͞W̸̛͉̮͖̑ͬͤͩͩͧ͑́̐ͅỤ̵̫͙̘̹̥͎̥̑̂̍̿ͤ̆͑̆̂ͪ͋͞͡͝͡Ẇ̨̡̹̭͈͎̮̜ͥ̂̄͂̌̒ͫͧͩ̋ͨͯ̃ͭ̌̃̓ͅŰ̶͖͉͍̼͔̪̙̼ͩ͗͆͂̽͋ͤ̓ͪ̀̅̅͐ͥ̚͟
</h3>
         </transition>
        <color-screen id='breath-display'
            :lightness='breath'
            :alpha='breath'
            @pointerdown='testTone'
        />
        <color-screen id='weather-display'
            :lightness='0'
            :hue='0'
            :saturation='0'
            :alpha='0'
            @pointerdown='testTone'
        />
        <connection-frame></connection-frame>
    </div>
</template>

<script>
import ColorScreen from '../components/user/ColorScreen';
import { useStore } from 'vuex';
import { computed, watch, ref } from 'vue';

import * as Tone from 'tone';
import NoSleep from 'nosleep.js'


export default {
    setup() {
        const store = useStore();
        let noSleep = new NoSleep();

        const breath = computed(() => store.getters.getBreath);
        const bite = computed(() => store.getters.getBite);
        const nod = computed(() => store.getters.getNod);
        const tilt = computed(() => store.getters.getTilt);
        const heartbeat = computed(() => store.getters.getHeartbeat);

        watch(breath, newValue => console.log('breath', newValue));
        watch(bite, newValue => console.log('bite', newValue));
        watch(nod, newValue => console.log('nod', newValue));
        watch(tilt, newValue => console.log('tilt', newValue));
        watch(heartbeat, newValue => console.log('heartbeat', newValue));



        // function mapRange(in1, in2, out1, out2, value) {
        //     return (value - in1) * (out2 - out1) / (in2 - in1) + out1;
        // }

        // const clamp = (num, min, max) => Math.min(Math.max(num, min), max);


        ////////////////TEST SYNTH///////////////
        const testSynth = new Tone.Synth().toDestination();

        function testTone() {
            const randomFrequency = 300 + Math.random() * 500;
            testSynth.triggerAttackRelease(randomFrequency,"8n");
        }







        const audioIsActive = ref(false);

        function startAudio() {
            audioIsActive.value = true;
            Tone.start()
            .then(() => {
                Tone.Transport.start();
            });
            store.dispatch('startAudio');
            noSleep.enable();
            document.querySelector('body').requestFullscreen();
        }

        return {
            startAudio,
            audioIsActive,
            testTone,
            breath
        }
    },
    components: {
        ColorScreen,
    },
    computed: {
        marqueeText() {
            if(this.audioIsActive) {
                return 'd(-.-)b d(-.-)b d(-.-)b d(-.-)b d(-.-)b '
            } else {
                return 'THISBODY THISBODY THISBODY '
            }
        }
    },
}
</script>

<style lang="stylus" scoped>
    .welcome
        height 100vh
        width 100vw
        background #000
        color #000
        position fixed
        top 0
        left 0
        // padding 50px
        box-sizing border-box
        display grid
        grid-template-rows 1fr 10fr

    .call-to-connect
        font-size 3.2vh
        text-align center
        font-family Comic-Sans
        hyphens auto
        margin 0
        grid-row 1/2
        padding 50px

    .cta
        text-shadow: 0 0 20px white,
                    0 0 20px white,
                    0 0 20px white;

    .welcome-transition-enter-from
        opacity 0
        transform scale(0.5)
    .welcome-transition-enter-to
        opacity 1
        transform scale(1)
    .welcome-transition-enter-active
        transition all 0.5s ease-out
    .welcome-transition-leave-from
        opacity 1
        transform scale(1)
    .welcome-transition-leave-to
        opacity 0
        transform scale(2)
    .welcome-transition-leave-active
        transition all 0.5s ease-in

    .yes-button
        justify-self center

    .now-touching
        font-size 3.8vh
        text-align center
        font-family sans-serif
        hyphens auto
        padding 50px
        position fixed
        top 20vh
        animation wiggle 10s infinite
        user-select none

    .title {
        font-size 4vh
        text-shadow: 0 0 20px white,
                    0 0 20px white,
                    0 0 20px white;
    }
    @keyframes wiggle {
        0% {
            transform: rotate(0deg);
        }
        33% {
            transform: rotate(10deg);
        }
        66% {
            transform: rotate(-10deg);
        }
        100% {
            transform: rotate(0deg);
        }
    }
</style>