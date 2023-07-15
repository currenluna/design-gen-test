<script lang="ts">
    
    import { onMount } from "svelte";
    import { jsPDF } from "jspdf";
    import videojs from "video.js";
    import WaveSurfer from "wavesurfer.js";
    import html2canvas from "html2canvas";
    
    import mp3 from "$lib/sounds/cage.mp3";
    // import { text } from "@sveltejs/kit";
    import video from "$lib/videos/sampl_2301_documentary-promo_v03.mp4"

    let waveformHTML: HTMLElement;
    let wavesurfer: WaveSurfer;
    let url: string;
    let title: string = "Hello World";

    onMount(() => {
        waveformHTML = document.querySelector("#waveform") as HTMLElement;
        wavesurfer = WaveSurfer.create({
            container: waveformHTML,
            height: 150,
            normalize: true,
            waveColor: '#000000',
            progressColor: '#000000',
            cursorWidth: 0,
            barWidth: 4,
            barGap: 4,
            barRadius: 10,
            url: video
        });

        // let wave;
        // let delay = setTimeout(() => {
        //     wave = wavesurfer.getWrapper().querySelector(".canvases")?.querySelector("div")?.querySelector("canvas");
        // }, 7000)

        // Select the node that will be observed for mutations
        const targetNode = wavesurfer.getWrapper();

        // Options for the observer (which mutations to observe)
        const config = { attributes: true, childList: true, subtree: true };

        // Callback function to execute when mutations are observed
        const callback = (mutationList: MutationRecord[], observer: MutationObserver) => {
            for (const mutation of mutationList) {
                if (mutation.type === "childList") {
                    console.log("A child node has been added or removed.");
                } else if (mutation.type === "attributes") {
                    console.log(`The ${mutation.attributeName} attribute was modified`);
                }
            }
        }

        // Create an observer instance linked to the callback function
        const observer = new MutationObserver(callback);

        // Start observing the target node for configured mutations
        observer.observe(targetNode, config);

        var c = document.getElementById("myCanvas") as HTMLCanvasElement;
        var ctx = c.getContext("2d")!;
        ctx.fillStyle = "white";
        ctx.fillRect(0, 0, c.width, c.height);
        ctx.fillStyle = "black";
        ctx.font = "500 40px Inter Variable";
        ctx.fillText(title, 10, 400);
        // $(onload) = () => {
        //     let lev1 = document.querySelector("#waveform")!;
        //     let lev2 = lev1.querySelector("div")!;
        //     let lev3 = lev2.shadowRoot!;
        //     let lev4 = lev3.querySelector("div")!;
        //     let lev5 = lev4.querySelector("div")!;
        //     let lev6 = lev5.querySelector("div")!;
        //     let lev7 = lev6.querySelector("div")!;
        //     console.log(lev6.childElementCount);
        // };



        // const myTimeout = setTimeout(() => {
        //     console.log(lev6.childElementCount);
        // }, 5000);
        // let canvas = lev7.querySelector("canvas")!;
        // url = canvas?.toDataURL("image/png")!;

        // console.log(canvas);
        // console.log(url);
    });

    const clearCanvas = () => {
        var c = document.getElementById("myCanvas") as HTMLCanvasElement;
        var ctx = c.getContext("2d")!;
        ctx.clearRect(0, 0, c.width, c.height);
    }

    const updateTitle = () => {
        clearCanvas();
        var c = document.getElementById("myCanvas") as HTMLCanvasElement;
        var ctx = c.getContext("2d")!;
        ctx.fillText(title, 10, 50);
    }


</script>

<main>
    <div id="waveform"></div>
    <canvas id="myCanvas" width="400" height="500"></canvas>
    <input type="text" bind:value={title} on:change={updateTitle} on:input={updateTitle}>
    <video src={video} id="myClip" playsinline></video>
</main>

<style>
    #myCanvas {
        border: 1px solid #000000;
    }
</style>


