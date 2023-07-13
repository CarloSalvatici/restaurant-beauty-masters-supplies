<script>
	import axios from "axios"
	import { onMount } from 'svelte'
	import localMenuData from '../public/menuData.json';
	import localAboutData from '../public/aboutData.json';
	import Menu from './sections/Menu.svelte'
	import Gallery from './sections/Gallery.svelte'
	import About from './sections/About.svelte'

	const url = 'https://carlo-web.herokuapp.com/api/accounts'

	let menuData = localMenuData
	let galleryData = []
	let aboutData = localAboutData

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

	let sections = {
		"About": 0,
		"Menu": 0,
		"Gallery": 0
	}
	const scrollToSection = (s) => {
		sections[s]
	}

</script>

<main>
	<header class="pt-5 px-1">
		<div class="title my-2 mb-4">
			<h1 class="title-text my-0 me-3"><strong>Test Website</strong></h1>
			<img class="logo-image" src="favicon.png" alt="Rigatini">
		</div>
		<h4 class="phone-number"><a href="tel:4692586258">(469) 258-6258</a></h4>
		<address>
			<h4 class=""><a target="_blank" and rel="noopener noreferrer" href="https://goo.gl/maps/RU3PW9xjWJwV444F7">1234 SW 1st St, Gainesville, FL</a></h4>
		</address>
		<h4 class="">Dine In or Carry Out from 11:00AM - 9:00PM</h4>
		<nav class="mt-5">
			<button on:click={() => {scrollToSection("Menu")}}>
				<h3 class="p-2 mb-0">Menu</h3>
			</button>
			<button on:click={() => {scrollToSection("Gallery")}}>
				<h3 class="p-2 mb-0">Gallery</h3>
			</button>
		</nav>
	</header>
	<div class="px-3 pb-3 content">
		<div class="section-label">
			
		</div>
		<About aboutData={aboutData}/>
		<h1 class="section-title">Menu</h1>
		<Menu menuData={menuData}/>
		<h1 class="section-title"></h1>
		<Gallery galleryData={galleryData}/>
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

	nav {
		display: flex;
		justify-content: space-evenly;
		border-top: 1px solid #F3F3F3;
	}

	nav > button {
		background: none;
		border: none;
	}
	nav > button > h3 {
		color: #F3F3F3;
	}
	nav > button:hover {
		background: rgba(255, 255, 255, 0.192);
		transition: .1s all;
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

	.section-title {
		border-bottom: 1px solid black;
		padding: 16px 0px 20px 0px !important;
		margin-bottom: none !important;
	}



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

	}
	/* Below 800px*/
	@media (max-width: 800px) {

	}

	/* Below 600px */
	@media (max-width: 600px) {

	}
</style>