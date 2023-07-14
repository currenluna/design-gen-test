<script lang="ts">
    const frameCount = new Array(72);
    
    import { onMount } from "svelte";
    import { jsPDF } from "jspdf";
    import WaveSurfer from "wavesurfer.js"
    import html2canvas from "html2canvas";
    
    import img from "$lib/images/michael.png"
    import mp3 from "$lib/sounds/cage.mp3"

    let doc: jsPDF;
    let elementHTML: HTMLElement;
    let waveformHTML: HTMLElement;
    let wavesurfer;

    onMount(() => {
        doc = new jsPDF();
        elementHTML = document.querySelector(".print") as HTMLElement;
        waveformHTML = document.querySelector("#waveform") as HTMLElement;
        wavesurfer = WaveSurfer.create({
            container: waveformHTML,
            height: 150,
            normalize: true,
            waveColor: '#000000',
            progressColor: '#000000',
            cursorWidth: 0,
            barWidth: 2,
            url: mp3
        });
        html2canvas(document.body, {useCORS: true}).then(canvas => {
            document.body.appendChild(canvas)
        });


        // var canvas = html2canvas(waveformHTML);
        // doc.addImage(canvas.toDataURL('image/png'));

    });

    
    const generatePDF = () => {	
        doc.html(elementHTML, {
            callback: function(doc) {
                // Save the PDF
                doc.save('document-html.pdf');
            },
            margin: [10, 10, 10, 10],
            autoPaging: 'text',
            x: 0,
            y: 0,
            width: 190, //target width in the PDF document
            windowWidth: 675 //window width in CSS pixels
        });
    }

</script>

<main>
    <div class="print">
        <div class="grid">
            {#each frameCount as frame}
                <img src={img} alt="">
            {/each}
        </div>
        <h1>Michael's Handshake</h1>
        <p>72 digital images and a waveform</p>
        <div id="waveform"></div>
    </div>
    <button on:click={generatePDF}>Generate</button>
</main>

<style>
    * {
        margin: 0;
        padding: 0;
    }
    .print {
        max-width: 1000px;
    }
    .grid {
        display: grid;
        grid-template-columns: repeat(12, 1fr);
        gap: .5rem;
    }
    img {
        width: 100%;
        object-fit: cover;
    }
    h1 {
        text-align: left;
    }

    button {
        margin: 2rem 0;
    }

    #waveform {
        min-width: 100px;
        min-height: 100px;
    }
    

</style>