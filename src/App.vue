<template>
    <div id='app'>
        <Envelope class="envelope"/>
        <Card class="card" v-if="(windowWidth > 1024)"></Card>
        <CardPortrait v-else class="card"/>
    </div>
</template>

<script>
    import Envelope from './assets/envelope.svg?inline';
    import Card from './assets/card.svg?inline';
    import CardPortrait from './assets/card-portrait.svg?inline';

    import gsap from 'gsap';
    import MorphSVGPlugin from 'gsap/MorphSVGPlugin.js';
    import MotionPathPlugin from 'gsap/MotionPathPlugin.js';
    import {SlowMo} from "gsap/EasePack";
    import {GSDevTools} from "gsap/GSDevTools";

    gsap.registerPlugin(MorphSVGPlugin, SlowMo, MotionPathPlugin, GSDevTools);


    export default {
        name: 'app',
        data() {
            return {
                windowWidth: window.innerWidth
            }
        },
        components: {
            Envelope,
            Card,
            CardPortrait
        },
        mounted() {


            function envelopeAnimation(envelopeWidth, finalWidth, finalX, finalY, url) {
                let envelope = document.querySelector('.envelope');


                function setCardPosition(id) {
                    let card = document.querySelector('.card');

                    let cardPositionWidth = document.querySelector('#card-position').getBoundingClientRect().width;
                    let cardPositionPortraitWidth = document.querySelector('#card-position-portrait').getBoundingClientRect().width;


                    let fromElement = document.querySelector(".card"),
                        toElement = document.querySelector(id),
                        point = {x: 0, y: 0},
                        convertedPoint = convertForTransforms(
                            fromElement,
                            toElement,
                            point,
                            {x: 0, y: 0}
                        );


                    function convertForTransforms(fromElement, toElement, fromPoint, toPoint) {
                        let parent = fromElement.parentNode,
                            p1 = MotionPathPlugin.convertCoordinates(fromElement, parent, fromPoint),
                            p2 = MotionPathPlugin.convertCoordinates(toElement, parent, toPoint);
                        console.log('convert for transforms');

                        return {x: p2.x - p1.x, y: p2.y - p1.y};

                    }

                    if (window.innerWidth < 1024) {
                        gsap.set(card, {x: convertedPoint.x, y: convertedPoint.y, width: cardPositionPortraitWidth});

                    } else {
                        gsap.set(card, {x: convertedPoint.x, y: convertedPoint.y, width: cardPositionWidth});

                    }
                }


                function changexlinkhref(value) {
                    document.querySelector(".card image").setAttributeNS('http://www.w3.org/1999/xlink', 'xlink:href', value);
                }

                changexlinkhref(url);
                // GSDevTools.create();
                const tlStart = gsap.timeline();
                const tlOpen = gsap.timeline({paused: true});

                if (window.innerWidth > 1024) {
                    gsap.set(envelope, {width: envelopeWidth, x: 0, y: 0});
                    setCardPosition("#card-position");

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

                        }, '-=1')


                        .add('show-paper')
                        .to('.envelope', 2.49, {attr: {viewBox: '0 0 700 3000'}}, 'show-paper')
                        .to(['#pattern-top', '#closed', '#shadows-inner', '#pattern-bottom', '#accents', '#body', '#bottom-shadow'], {
                            y: '+=250px',

                            duration: 2.49


                        }, 'show-paper')
                        .to('#paper-mask', {y: '250px', duration: 2.49}, 'show-paper')

                        .to('.envelope', {

                            y: '+=500',
                            autoAlpha: 0

                        })
                        .to('.card', {width: finalWidth, x: finalX, y: finalY})


                } else if (window.innerWidth < 1024) {
                    gsap.set(envelope, {width: envelopeWidth, x: 0, y: 0});
                    setCardPosition("#card-position-portrait");

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
                        .from('#paper-portrait', {
                            duration: 2,
                            scaleY: 0,
                            transformOrigin: 'center bottom'

                        }, '-=1')


                        .add('show-paper')
                        .to('.envelope', 2.49, {attr: {viewBox: '0 0 700 3000'}}, 'show-paper')
                        .to(['#pattern-top', '#closed', '#shadows-inner', '#pattern-bottom', '#accents', '#body', '#bottom-shadow'], {
                            y: '+=300px',

                            duration: 2.49


                        }, 'show-paper')
                        .to('#paper-mask-portrait', {y: '300px', duration: 2.49}, 'show-paper')

                        .to('.envelope', {

                            y: '+=500',
                            autoAlpha: 0

                        })
                        .to('.card', {width: finalWidth, x: finalX, y: finalY})
                }


                tlStart
                    .from('#text > *', {autoAlpha: 0, stagger: 0.1})
                    .from('#arrow', {y: '+=10', repeat: 10, yoyo: true, autoAlpha: 0});
                const button = document.querySelector('#app');

                button.addEventListener('click', function () {
                    tlOpen.play();
                    tlStart.progress(0).pause();
                })
            }

            envelopeAnimation(300, 400, 900, 0, 'https://via.placeholder.com/860x645');
            // envelopeAnimation(300, 350, 0, 0, 'https://via.placeholder.com/720x1280');


        }
    }

</script>

<style>
    #closed-shadows, #opened-top, #shadow-paper, #paper-to, #card-position, #card-position-portrait {
        visibility: hidden;
    }

    * {
        box-sizing: border-box;
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
        position: relative;

    }


    #app svg {

        pointer-events: none;
    }


    .card {
        position: absolute;
        left: 0;
        top: 0;
        height: auto;
        transform: translate(0, 0);

    }

    .envelope {
        position: absolute;
        left: 0;
        top: 0;
        width: 300px;
        height: auto;

    }


</style>
