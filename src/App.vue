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

            // function setSvgAttrs() {
            //     let svg = document.getElementsByTagName('svg')[0];
            //
            //     const background01 = document.querySelector('#closed');
            //
            //
            //     function createFilter() {
            //         const svgns = "http://www.w3.org/2000/svg";
            //
            //         let filter = document.createElementNS(svgns, "filter");
            //         let defs = document.createElementNS(svgns, "defs");
            //         let gaussian = document.createElementNS(svgns, "feGaussianBlur");
            //         gaussian.setAttribute("stdDeviation", '3 1 ');
            //         filter.setAttribute("id", "blur-image-effect");
            //
            //         svg.appendChild(defs);
            //         defs.appendChild(filter);
            //         filter.appendChild(gaussian);
            //     }
            //
            //     function applyFilter(elem, filterId) {
            //         elem.style.filter = "url(#" + filterId + ")";
            //         return elem;
            //     }
            //
            //     createFilter();
            //     applyFilter(background01, 'blur-image-effect');
            // }

            // setSvgAttrs();

            GSDevTools.create({timeline: tlOpen});
            // const tlStart = gsap.timeline();
            const tlOpen = gsap.timeline();
            MorphSVGPlugin.convertToPath('circle, rect, ellipse, line, polygon, polyline');
            tlOpen

                .from('#text > *', {autoAlpha: 0, stagger: 0.1})
                .from('#arrow', {y: '+=10', repeat: 2, yoyo: true, autoAlpha: 0})
                .to('#arrow', {autoAlpha: 0})
                .to('#button', {autoAlpha: 0, scale: 0, transformOrigin: 'center center'}, '<')
                .to('#text > *', {autoAlpha: 0, stagger: 0.1}, '<')

                .to('#closed', {
                    duration: 2,
                    transformOrigin: 'center top',
                    fill: '#f5f5f5',
                    scaleY: -1,


                })
                // .to('#filter-drop', 0.5, {attr: {stdDeviation: 0, dy: 0}}, '<')
                .from('#pattern-top', {
                    duration: 1.3,
                    transformOrigin: 'center bottom',
                    scaleY: 0,
                    // y: '+=6'
                }, '<0.6')
                .from('#paper', {
                    duration: 2,
                    scaleY: 0,
                    transformOrigin: 'center bottom'

                }, '-=2.2')


                .to('#paper', {y: '-=85'}, '<')
                .add('show-paper', '-=1')
                .to(['#pattern-top', '#closed', '#shadows-inner', '#pattern-bottom', '#accents', '#body', '#bottom-shadow'], {
                    y: '+=860',
                    duration: 2.54,

                    autoAlpha: 0
                }, 'show-paper')
                .to('#paper-mask', {y: '+=340', duration: 0.86}, 'show-paper')


                // .to('#animation > g:nth-child(11)', {
                //
                //     x: '+=250',
                //     y: '+=40',
                //     transformOrigin: 'center center'
                // }, '<0.9')
                .add('show-final', '-=1')
                .from('#paper-mask-full', {autoAlpha: 0, duration: 0.01}, 'show-final')
                .to('#paper', {
                    duration: 1,
                    stroke: 'transparent',
                    fill: '#f49494',
                    x: '+=280',
                    y: '+=80',
                    morphSVG: {
                        shape: "#paper-morph-to",
                        type: "linear",
                        shapeIndex: "2"
                    }
                }, 'show-final')


                .fromTo('#envelope-half', {x: '-=1500', duration: 5}, {x: -1050}, 'show-final')

                .to('#envelope-half', {x: '-=1050', delay: 0.5})
                .to('#paper', {x: 0}, '<')


                .from('#shadows-inner', {autoAlpha: 0, y: '+=2'}, 0.1);


            // tlStart
            //     .from('#text > *', {autoAlpha: 0, stagger: 0.1})
            //     .from('#arrow', {y: '+=10', repeat: 2, yoyo: true, autoAlpha: 0})
            // ;


            // const button = document.querySelector('#app');
            //
            // button.addEventListener('click', function () {
            //     tlOpen.play();
            //     tlStart.progress(0).pause();
            // })
        }
    }

</script>

<style>
    #closed-shadows, #opened-top, #shadow-paper, #paper-morph-to {
        visibility: hidden;
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
        display: grid;
        justify-content: center;
        align-content: center;
    }


    #app svg {
        height: 95vh;
        width: 100vw;
        pointer-events: none;
    }
</style>
