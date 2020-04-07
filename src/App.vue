<template>
    <div id='app'>
        <Envelope class="envelope"/>
        <Card class="card"/>
    </div>
</template>

<script>
    import Envelope from './assets/envelope.svg?inline';
    import Card from './assets/card.svg?inline';

    import gsap from 'gsap';
    import MorphSVGPlugin from 'gsap/MorphSVGPlugin.js';
    import MotionPathPlugin from 'gsap/MotionPathPlugin.js';
    import {SlowMo} from "gsap/EasePack";
    import {GSDevTools} from "gsap/GSDevTools";

    gsap.registerPlugin(MorphSVGPlugin, SlowMo, MotionPathPlugin, GSDevTools);


    export default {
        name: 'app',

        components: {
            Envelope,
            Card
        },
        mounted() {


            function envelopeAnimation() {


                let widthEnvelope = 600;


                let envelope = document.querySelector('.envelope');
                let card = document.querySelector('.card');


                function percentage(percent, total) {
                    return ((percent / 100) * total).toFixed(2)
                }


                const percentOffsetOfCardWidth = percentage(12, widthEnvelope);
                let widthCard = widthEnvelope - percentOffsetOfCardWidth;

                let fromElement = document.querySelector(".card"),
                    toElement = document.querySelector("#card-position"),
                    point = {x: 0, y: 0},
                    convertedPoint = convertForTransforms(
                        fromElement,
                        toElement,
                        point,
                        {x: -20, y: 0}
                    );


                // returns the relative distance on the x and y axis.
                function convertForTransforms(fromElement, toElement, fromPoint, toPoint) {
                    var parent = fromElement.parentNode,
                        p1 = MotionPathPlugin.convertCoordinates(fromElement, parent, fromPoint),
                        p2 = MotionPathPlugin.convertCoordinates(toElement, parent, toPoint);
                    return {x: p2.x - p1.x, y: p2.y - p1.y};
                }

                gsap.set(envelope, { width: widthEnvelope});
                gsap.set(card, {x: convertedPoint.x, y: convertedPoint.y, width: widthCard});
                // console.log(convertedPoint);

                // function changexlinkhref(value) {
                //     document.querySelector("image").setAttributeNS('http://www.w3.org/1999/xlink', 'xlink:href', value);
                // }

                // changexlinkhref(url);
                GSDevTools.create();
                const tlStart = gsap.timeline();
                const tlOpen = gsap.timeline();
                tlOpen

                // .to(envelope, {x: 0, y: 0})
                // .to(card, {x: 0, y: 0})
                ;
                // .to('#arrow', {autoAlpha: 0})
                // // .to('#button', {autoAlpha: 0, scale: 0, transformOrigin: 'center center'}, '<')
                // .to('#closed', {
                //     duration: 2,
                //     transformOrigin: 'center top',
                //     fill: '#f5f5f5',
                //     scaleY: -1,
                //
                //
                // })
                // .from('#pattern-top', {
                //     duration: 1.3,
                //     transformOrigin: 'center bottom',
                //     scaleY: 0,
                // }, '<0.6')
                // .from('#paper', {
                //     duration: 2,
                //     scaleY: 0,
                //     transformOrigin: 'center bottom'
                //
                // }, '-=1')
                //
                //
                //
                //
                // .add('show-paper')
                // .to(['#pattern-top', '#closed', '#shadows-inner', '#pattern-bottom', '#accents', '#body', '#bottom-shadow'], {
                //     y: '+=250',
                //     duration: 2.49
                //
                //
                // }, 'show-paper')
                // .to('#paper-mask', {y: '250', duration: 2.49}, 'show-paper')
                //
                // .to('.envelope', {
                //     x: 0,
                //     y: 500,
                //
                //     scale: 1
                //
                // })


                // tlStart
                //     .from('#text > *', {autoAlpha: 0, stagger: 0.1})
                //     .from('#arrow', {y: '+=10', repeat: 10, yoyo: true, autoAlpha: 0});
                const button = document.querySelector('#app');

                button.addEventListener('click', function () {
                    tlOpen.play();
                    tlStart.progress(0).pause();
                })
            }

            envelopeAnimation();


        }
    }

</script>

<style>
    #closed-shadows, #opened-top, #shadow-paper, #paper-to {
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

    /*.card {*/
    /*    position: absolute;*/
    /*    left: 50%;*/
    /*    top: 50.5%;*/
    /*    width: 539px;*/
    /*    height: auto;*/
    /*    transform: translate(-50%, -50%);*/
    /*}*/

    /*.envelope {*/
    /*    position: absolute;*/
    /*    left: 50%;*/
    /*    top: 50%;*/
    /*    width: 600px;*/
    /*    height: auto;*/
    /*    transform: translate(-50%, -50%);*/

    /*}*/


    .card {
        position: absolute;
        left: 0;
        top: 0;
        width: 539px;
        height: auto;
        /*transform: translate(-50%, -50%);*/

    }

    .envelope {
        position: absolute;
        left: 0;
        top: 0;
        width: 600px;
        height: auto;

    }


</style>
