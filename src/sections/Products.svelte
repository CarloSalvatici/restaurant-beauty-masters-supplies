<script>
	import { onMount } from 'svelte'
    export let products
    
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
    <div id="products" class="products">
        <div class="products-category-contents">
            <div class="mc-item-list">
                {#each products as data}
                        <div class="mc-item py-3">
                            <h3>{data.name}</h3>
                            <span>{data.description}</span>
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

	.products {
		display: flex;
	}

	.products-category-titles {
		position: relative;
	}

	.products-category-contents {
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
		.products-category-titles {
			min-width: 300px;
			max-width: 300px;
		}
	}

	@media (min-width: 1500px) {
		.products-category-titles {
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
		.products {
			display: block;
		}
		.products-category-titles {
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