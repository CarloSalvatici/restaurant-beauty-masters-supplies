<script>
	import { onMount } from 'svelte'
    export let menu
    
    $: innerWidth = 0

	let mc_img
	onMount(async () => {
        mc_img = document.getElementById("mc-img-id")
        document.addEventListener("scroll", scrollMcImg)
	})
    
    const scrollMcImg = () => {
        mc_img.style.transform = "translateX(-100px) translateY(" + (scrollY*(.25) - 300) + "px)"
    }

    // This function is for toggling to show the list of selectable categories on mobile
	let displayCategories = false
	const mcToggle = () => {
		displayCategories = !displayCategories
	}

	// This function is for changing the selected category
	let displayedCategory = 0
	const changeCategory = (index) => {
		displayedCategory = index
		displayCategories = false
	}
</script>

<svelte:window bind:innerWidth />

<main>
    <div id="menu" class="menu">
        <div class="menu-category-titles">
			<div class="mc-img">
				<img id="mc-img-id" src="images/decorative/vine.png" alt="vine"/>
			</div>
            {#if innerWidth < 700}
                <button class="mc-toggle py-1 px-3" on:click={mcToggle}>
                    <h2 class="mt-2">
                        {#if displayCategories == true}Collapse Categories <img class="mc-expand" src="./images/expand-arrow-up.png" alt="Expand Arrow">{/if}
                        {#if displayCategories == false}View Categories <img class="mc-expand" src="./images/expand-arrow-down.png" alt="Expand Arrow">{/if}
                    </h2>
                </button>
            {/if}
            {#if displayCategories == true || innerWidth > 699}
                {#each menu as data}
                    <button class="mc-title p-1 pb-2 mt-1 {displayedCategory === menu.indexOf(data) ? 'selected' : ''}" on:click={() => changeCategory(menu.indexOf(data))}>
                        <h2 class="mb-0">{data.categoryTitle}</h2>
                    </button>
                {/each}
            {/if}
        </div>
        <div class="menu-category-contents">
            {#if innerWidth < 701}
                <h2 class="mc-mobile-title pt-4 mb-0">{menu[displayedCategory].categoryTitle}</h2>
            {/if}
            {#if menu[displayedCategory].description != undefined && menu[displayedCategory].description != ""}
                <span>{menu[displayedCategory].description}</span>
                <hr>
            {/if}
            <div class="mc-item-list">
                {#each menu[displayedCategory].items as data}
                        <div class="mc-item py-3">
                            <h3>{data.name}</h3>
                            <span>{data.description} {data.price}</span>
                            {#if data.vegan} <img src="./images/veganSm.png" alt="Vegan"> {/if}
                        </div>
                {/each}
            </div>
        </div>
    </div>
</main>

<style>

    h2 {
		font-size: 2em;
	}

	.menu {
		display: flex;
	}

	.menu-category-titles {
		position: relative;
	}

	.menu-category-contents {
		flex-grow: 1;
	}

	.mc-img {
		position: absolute;
		overflow: hidden;
		width: 100%;
		height: 100%;
		z-index: 0;
	}
	.mc-img > img {
		width: 150%;
		transform: translateX(-100px);
	}

	.mc-toggle:hover {
		background: var(--category-expand-bg-color-hover);
		transition: .3s all;
		cursor: pointer;
	}

	.mc-toggle { 
		position: relative;
		border: 2px black solid;
		background: none;
		width: fit-content;
		margin: auto;
		z-index: 5;
	}

	.mc-expand {
		width: 23px;
		position: relative;
		transform: translateY(-5%);
	}

	.mc-title:nth-child(odd) {
		background: var(--mc-title-alternating-bg);
	}

	.mc-title {
		position: relative;
		display: block;
		border: none;
		background: none;
		z-index: 5;
	}

	.selected {
		border: 1px var(--ct-border-color) solid;
		background: var(--ct-bg-color) !important;
		font-weight: bold;
	}

	.mc-title:hover {
		background: var(--ct-bg-color);
		transition: .3s all;
		cursor: pointer;
	}

	.mc-title {
		transition: .3s all;
	}

	.mc-item-list {
		display: flex;
		justify-content: center;
		flex-direction: row;
		flex-flow: wrap;
		padding-bottom: 1em;
	}

	.mc-item > img {
		width: 25px;
		height: auto;
	}

	.mc-item {
		margin: .3em;
		padding: 6px;
		height: auto;
		text-align: center;
	}

	@media (max-width: 1499px){
		.menu-category-titles {
			min-width: 300px;
			max-width: 300px;
		}
	}

	@media (min-width: 1500px) {
		.menu-category-titles {
			min-width: 400px;
			max-width: 400px;
		}
	}

	@media (min-width: 1398px) {
		.mc-item {
			width: 400px;
		}
	}
	@media (max-width: 1397px) {
		.mc-item {
			width: 100%;
		}
	}

	@media (max-width: 699px) {
		.menu {
			display: block;
		}
		.menu-category-titles {
			margin: auto;
			min-width: 100%;
			max-width: 100%;
		}
		.mc-title {
			margin: auto;
			width: 75%;
		}
	}
	@media (min-width: 700px) {
		.mc-title {
			text-align: left;
			width: 90%;
		}
	}

</style>