<script>
    export let pageData
    import { onMount } from 'svelte'

    $: innerWidth = 0
    $: scrollY = 0
    let about_txt
    onMount(async () => {
        about_txt = document.getElementById("about-scroll-id")
        document.addEventListener("scroll", scrollParagraph)
	})
    
    const scrollParagraph = () => {
        about_txt.style.transform = "translatex(-30px) translateY(" + (scrollY*(-.25) + 100) + "px)"
    }
</script>

<svelte:window bind:scrollY={scrollY} bind:innerWidth={innerWidth} />

<main id="about">
    <div class="about p-2  {innerWidth <= 1100 ? 'mb-6' : ''}">
        <img class="about-img" src="images/italian-family.jpg" alt="gif"/>
        <div class="about-text p-3" id="about-scroll-id">
            <h1 class="p-1"><i>{pageData.aboutTitle}</i></h1>
            <p>{pageData.aboutParagraph}</p>
        </div>
    </div>
</main>

<style>
    .about {
        display: flex;
    }
    .about-img {
        width: 50vw;
        height: fit-content;
        z-index: 0;
    }
    .about-text {
        min-height: 0;
        height: 100%;
        background: #ffffff;
        transform: translate(-30px, 100px);
        border: 1px solid grey;
        z-index: 2;
    }
    .about-text > h1 {
        text-align: left;
    }
    .about-text > p {
        text-align: left;
        font-size: 1em;
    }
    .mb-6 {
        margin-bottom: 80px;
    }
    /* Below 800px*/
	@media (max-width: 800px) {
        .about {
            display: block;
        }
        .about-img {
            width: 100%;
            height: auto;
            overflow: hidden;
            z-index: 0;
        }
        .about-text{
            margin-left: 30px;
            margin-top: -75px;
        }
    }
</style>