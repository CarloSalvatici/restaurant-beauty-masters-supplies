<script>
    export let aboutData
    import { onMount } from 'svelte'

    let scrollY
    let about_txt
    onMount(async () => {
        about_txt = document.getElementById("about-scroll-id")
	})
    
    const scrollParagraph = () => {
        console.log("fuck")
        about_txt.style.transform = translateY(scrollY*.2)
    }
</script>

<svelte:window bind:scrollY={scrollY} />

<main>
    <div class="about p-2">
        <img class="about-img" src="images/italian-family.jpg" alt="gif" />
        <div on:scroll={scrollParagraph} class="about-text p-3" id="about-scroll-id">
            <h1 class="p-1"><i>Ciao!</i></h1>
            <p>{aboutData.mainParagraph}</p>
        </div>
    </div>
</main>

<style>
    .about {
        display: flex;
        transform-style: preserve-3d;
    }
    .about-img {
        width: 50vw;
    }
    .about-text {
        min-height: 0;
        height: 100%;
        background: #ffffff;
        transform: translate(-30px, 0px);
        transform: translateZ(-10px);
    }
    .about-text > h1 {
        text-align: left;
    }
    .about-text > p {
        text-align: left;
        font-size: 1em;
    }
</style>