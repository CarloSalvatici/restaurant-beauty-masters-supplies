<script>
    export let menuData
    
    $: innerWidth = 0

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
            {#if innerWidth < 700}
                <button class="mc-toggle py-1 px-3" on:click={mcToggle}>
                    <h2 class="mt-2">
                        {#if displayCategories == true}Collapse Categories <img class="mc-expand" src="./images/expand-arrow-up.png" alt="Expand Arrow">{/if}
                        {#if displayCategories == false}View Categories <img class="mc-expand" src="./images/expand-arrow-down.png" alt="Expand Arrow">{/if}
                    </h2>
                </button>
            {/if}
            {#if displayCategories == true || innerWidth > 699}
                {#each menuData as data}
                    <button class="mc-title p-1 pb-2 mt-1 {displayedCategory === menuData.indexOf(data) ? 'selected' : ''}" on:click={() => changeCategory(menuData.indexOf(data))}>
                        <h2 class="mb-0">{data.categoryTitle}</h2>
                    </button>
                {/each}
            {/if}
        </div>
        <div class="menu-category-contents">
            {#if innerWidth < 701}
                <h2 class="mc-mobile-title pt-4 mb-0">{menuData[displayedCategory].categoryTitle}</h2>
            {/if}
            {#if menuData[displayedCategory].description != undefined && menuData[displayedCategory].description != ""}
                <span>{menuData[displayedCategory].description}</span>
                <hr>
            {/if}
            <div class="mc-item-list">
                {#each menuData[displayedCategory].items as data}
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

	.menu-category-contents {
		flex-grow: 1;
	}

	.mc-toggle:hover {
		background: rgba(220, 219, 168, 0.4);
		transition: .3s all;
		cursor: pointer;
	}

	.mc-toggle {
		border: 2px black solid;
		background: none;
		width: fit-content;
		margin: auto;
	}

	.mc-expand {
		width: 23px;
		position: relative;
		transform: translateY(-5%);
	}

	.mc-title:nth-child(odd) {
		background: #f7f7f7;
	}

	.mc-title {
		display: block;
		border: none;
		background: none;
	}

	.selected {
		border: 1px #e60004 solid;
		background: rgba(233, 186, 186, 0.4) !important;
		font-weight: bold;
	}

	.mc-title:hover {
		background: rgba(233, 186, 186, 0.4);
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
		border: 1px all black;
	}

	@media (min-width: 1398px) {
		.mc-item{
			width: 400px;
		}
	}
	@media (max-width: 1397px) {
		.mc-item{
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