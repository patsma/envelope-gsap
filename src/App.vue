<template>
    <div id='app'>
        <Envelope/>
    </div>
</template>

<script>
    import Envelope from './assets/envelope.svg?inline';

    import gsap from 'gsap';
    import MorphSVGPlugin from 'gsap/MorphSVGPlugin.js';
    import {SlowMo} from "gsap/EasePack";
    import {GSDevTools} from "gsap/GSDevTools";

    gsap.registerPlugin(MorphSVGPlugin, SlowMo, GSDevTools);


    export default {
        name: 'app',

        components: {
            Envelope
        },
        mounted() {




            function envelopeAnimation(positionX, positionY, scale) {

                GSDevTools.create();
                const tlStart = gsap.timeline();
                const tlOpen = gsap.timeline({paused: true});
                MorphSVGPlugin.convertToPath('circle, rect, ellipse, line, polygon, polyline');
                tlOpen

                    .to('#arrow', {autoAlpha: 0})
                    .to('#button', {autoAlpha: 0, scale: 0, transformOrigin: 'center center'}, '<')
                    .to('#closed', {
                        duration: 2,
                        transformOrigin: 'center top',
                        fill: '#f5f5f5',
                        scaleY: -1,


                    })
                    .from('#pattern-top', {
                        duration: 1.3,
                        transformOrigin: 'center bottom',
                        scaleY: 0,
                    }, '<0.6')
                    .from('#paper', {
                        duration: 2,
                        scaleY: 0,
                        transformOrigin: 'center bottom'

                    }, '-=2.2')


                    .to('#paper', {y: '-=85'}, '<')
                    .set('#mask', {display: 'none'})

                    .add('show-paper', '-=1')
                    .to(['#pattern-top', '#closed', '#shadows-inner', '#pattern-bottom', '#accents', '#body', '#bottom-shadow'], {
                        y: '+=960',
                        duration: 2.49,

                        autoAlpha: 0
                    }, 'show-paper')
                    .to('#paper-mask', {y: '+=280', duration: 0.5}, 'show-paper')

                    .add('show-final', '-=1')
                    .to('#paper', {
                        x: positionX,
                        y: positionY,
                        scale: scale,
                        transformOrigin: 'center center'

                    }, 'show-final')


                    .from('#paper-mask-full', {autoAlpha: 0, duration: 0.01}, 'show-final')
                    .fromTo('#envelope-half', {xPercent: '-=120'}, {xPercent: '-=120'}, 'show-final')


                    .to('#envelope-half', {xPercent: '-=100', duration: 2, delay: 1})
                    // .to('#paper', {x: 0, y: -20}, '<')
                    .from('#shadows-inner', {autoAlpha: 0, y: '+=2'}, 0.1);


                tlStart
                    .from('#text > *', {autoAlpha: 0, stagger: 0.1})
                    .from('#arrow', {y: '+=10', repeat: 10, yoyo: true, autoAlpha: 0});
                const button = document.querySelector('#app');

                button.addEventListener('click', function () {
                    tlOpen.play();
                    tlStart.progress(0).pause();
                })
            }

            envelopeAnimation(600, -100, 1);


        }
    }

</script>

<style>
    #closed-shadows, #opened-top, #shadow-paper, #paper-to {
        visibility: hidden;
    }

    #paper {
        stroke: transparent;
    }

    body {
        margin: 0;
        padding: 0;
    }


    html {
        background-color: beige;
        padding: 0;
        margin: 0;
        overflow: hidden;

    }

    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        min-height: 100vh;
        display: flex;
        justify-content: center;
        align-content: center;
        width: 100vw;

    }


    /*#app svg {*/
    /*    height: 95vh;*/
    /*    width: 100vw;*/
    /*    pointer-events: none;*/
    /*} */
    #app svg {
        height: 100vh;
        width: 100%;
        pointer-events: none;
    }
</style>
