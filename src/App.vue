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
            GSDevTools.create({minimal: true});
            const tl = gsap.timeline();
            MorphSVGPlugin.convertToPath('circle, rect, ellipse, line, polygon, polyline');
            // findShapeIndex("#top-open > path", "#top-closed > path");
            tl

                .to('#closed-path', {
                    duration: 2,
                    transformOrigin: 'center top',
                    fill: '#f5f5f5',
                    scaleY: -1
                    // morphSVG: {
                    //     shape: '#opened-path',
                    //     ease: 'slow(1.1,2.5)',
                    //     type: 'rotational',
                    //     shapeIndex: 1
                    // }

                })
                .from('#pattern', {
                    duration: 1.3,
                    transformOrigin: 'center bottom',
                    scaleY: 0
                }, '<0.7')

        }
    }

</script>

<style>
    #closed-shadows
    , #shadow-paper {
        visibility: hidden;
    }


    html {
        background-color: black;
        padding: 0;
        margin: 0;
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
        height: 80vh;
        pointer-events: none;
    }
</style>
