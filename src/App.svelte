<script>
	import axios from "axios"
	import { onMount } from 'svelte'
	import localMenuData from '../public/menuData.json';
	import Menu from './sections/Menu.svelte'
	import Gallery from './sections/Gallery.svelte'

	$: innerWidth = 0
	const url = 'https://carlo-web.herokuapp.com/api/accounts'

	let menuData = localMenuData
	let galleryData = []
	console.log(menuData)

	onMount(async () => {
		try {
			let res = await axios.get(url, {params: {getType: "getBusinessDataOf", businessName: "Test Document"}})
			menuData = res.data.businessData.menu
			galleryData = res.data.businessData.gallery
			console.log(res.data.msg)

		} catch(err) {
			console.log(err)
		}
	})

	// This function controls the scrolling between menu and gallery
	let section = 0
	let sectionCount = 2
	let sectionTitleList = ["Menu", "Gallery"]
	// Direction should be 1 for right and -1 for left
	const sectionScroll = (direction) => {
		section += direction
		// Absolute value of section mod sectionCount so that the scroll loops around properly
		section = Math.abs(section%sectionCount)
		console.log(section)
	}

</script>

<svelte:window bind:innerWidth />

<main>
	<header class="py-5">
		<div class="title my-2 mb-4">
			<h1 class="title-text my-0 me-3"><strong>Test Website</strong></h1>
			<img class="logo-image" src="favicon.png" alt="Rigatini">
		</div>
		<h4 class="phone-number"><a href="tel:4692586258">(469) 258-6258</a></h4>
		<address>
			<h4 class=""><a target="_blank" and rel="noopener noreferrer" href="https://goo.gl/maps/RU3PW9xjWJwV444F7">1234 SW 1st St, Gainesville, FL</a></h4>
		</address>
		<h4 class="">Dine In or Carry Out from 11:00AM - 9:00PM</h4>
	</header>
	<div class="p-3 content">
		<div class="section-label">
			<!--Left and right arrows <button class="sl-left-arrow" on:click={() => {sectionScroll(-1)}}>
				<img src="./images/simple-arrow-left.png" alt="left arrow"/>
			</button>
			<button class="sl-right-arrow" on:click={() => {sectionScroll(1)}}>
				<img src="./images/simple-arrow-right.png" alt="right arrow"/>
			</button>-->
			<img src="./images/giphy.webp" alt="Click Gif"/>
			<button on:click={() => {sectionScroll(1)}}>
				<h1 class="section-title">{sectionTitleList[section]}</h1>
			</button>
		</div>
		{#if section == 0}
			<Menu menuData={menuData}/>
		{/if}
		{#if section == 1}
			<Gallery galleryData={galleryData}/>
		{/if}
	</div>
</main>

<style>
	@font-face{
		font-family: thefontimusing;
		src: url(../fonts/Manrope-Regular.ttf);
		font-weight: normal;
		font-style: normal;
	}
	@font-face{
		font-family: thefontimusing;
		src: url(../fonts/Manrope-Bold.ttf);
		font-weight: bold;
		font-style: normal;
	}

	main {
		text-align: center;
		margin: 0 auto;
	}


	header {
		font-family: thefontimusing;
		background: black;
		color: #F3F3F3;
		width: 100%;
	}
	
	button {
		background: none;
		border: none;
	}

	h1 {
		text-transform: uppercase;
		font-size: 5em;
	}


	h4 > a {
		text-decoration: none;
		color: #F3F3F3;
		transition: all .3s;
	}

	h4 > a:hover{
		color: #457B9D;
	}

	header > address {
		margin: auto;
	}

	.title-text {
		display: inline-block;
		vertical-align: middle;
		color: #e6c11d;
		font-family:'Times New Roman', Times, serif;
	}
    /*EF233C*/

	.logo-image {
		display: inline-block;
		width: 110px;
		height: auto;
		vertical-align: middle;
	}

	.content {
		text-align: center;
		margin: 0 auto;
		font-family: thefontimusing;
		background: #ffffff;
	}

	.section-label {
		position: relative;
		margin:0;
		margin-bottom: 15px;
	}
	.section-label > button {
		padding: none;
		width: 100%;
		background: none;
		border: none;
	}
	.section-label > img {
		position: absolute;
		left: 60%;
		top: 50%;
		transform: translate(-50%, -50%);
		width: 75px;
	}

	.section-title {
		border-bottom: 1px solid black;
		padding: 0px 0px 20px 0px !important;
		margin-bottom: none !important;
	}
	.section-title:hover {
		background: rgba(220, 219, 168, 0.4);
		transition: .3s all;
	}

	/* .sl-left-arrow{
		position: absolute;
		left: 10%;
		padding: 25px 125px 25px 125px;
	}

	.sl-right-arrow{
		position: absolute;
		right: 10%;
		padding: 25px 125px 25px 125px;
	} */

	 /*menu takes up not the whole screen on larger window*/
	 @media (max-width: 1649px) {
		.content {
			max-width: 100%
		}
	}

	@media (min-width: 1650px) {
		.content {
			max-width: 80%
		}	
	}
	/* Below 1250px*/
	@media (max-width: 1250px) {
		.sl-left-arrow{
			position: absolute;
			left: 0;
			padding: 25px 100px 25px 100px;
		}
		.sl-right-arrow{
			position: absolute;
			right: 0;
			padding: 25px 100px 25px 100px;
		}
	}
	/* Below 800px*/
	@media (max-width: 800px) {
		.sl-left-arrow{
			position: absolute;
			left: 0;
			padding: 25px 50px 25px 50px;
		}
		.sl-right-arrow{
			position: absolute;
			right: 0;
			padding: 25px 50px 25px 50px;
		}
	}

	/* Below 600px */
	@media (max-width: 600px) {
		.sl-left-arrow{
			position: absolute;
			left: 0;
			padding: 25px;
		}
		.sl-right-arrow{
			position: absolute;
			right: 0;
			padding: 25px;
		}
	}
</style>