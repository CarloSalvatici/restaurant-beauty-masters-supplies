<script>
	import axios from "axios"
	import { onMount } from 'svelte'
	import localMenuData from '../public/menuData.json';
	import localAboutData from '../public/aboutData.json';
	import Menu from './sections/Menu.svelte'
	import Gallery from './sections/Gallery.svelte'
	import About from './sections/About.svelte'
	import Lower from './sections/Lower.svelte'

	const url = 'https://carlo-web.herokuapp.com/api/accounts'

	let scrollY
	let innerWidth

	let prevScroll = 0
	let scrollAnchor = 0
	let hideNav = true

	let menuData = localMenuData
	let galleryData = []
	let aboutData = localAboutData
	let navStickyPos = 1000;
	let loading = true;

	onMount(async () => {
		try {
			let res = await axios.get(url, {params: {getType: "getBusinessDataOf", businessName: "Test Document"}})
			menuData = res.data.businessData.menu
			galleryData = res.data.businessData.gallery
			console.log(res.data.msg)
			loading = false

		} catch(err) {
			console.log(err)
		}
		navStickyPos = document.getElementById("nav").offsetTop
		document.addEventListener("scroll", (event) => {
			if(scrollY > prevScroll) {
				scrollAnchor = scrollY
				hideNav = true
			} else {
				if(scrollAnchor - scrollY > 140){
					hideNav = false
				}
			}
			prevScroll = scrollY
		});
	})

	const scrollToSection = (id) => {
		scrollY = document.getElementById(id).offsetTop - 140
	}
</script>

<svelte:window bind:scrollY={scrollY} bind:innerWidth={innerWidth}/>

<main>
	<header class="pt-5 {scrollY >= navStickyPos ? 'sticky-header-padding' : ''}">
		<div class="title my-2 mb-4">
			<h1 class="title-text my-0 me-3"><strong>Test Website</strong></h1>
			<img class="logo-image" src="favicon.png" alt="Rigatini">
		</div>
		<h4 class="phone-number"><a href="tel:4692586258">(469) 258-6258</a></h4>
		<address>
			<h4 class=""><a target="_blank" and rel="noopener noreferrer" href="https://goo.gl/maps/RU3PW9xjWJwV444F7">1234 SW 1st St, Gainesville, FL</a></h4>
		</address>
		<h4 class="">Dine In or Carry Out from 11:00AM - 9:00PM</h4>
		<nav id="nav" class="{scrollY >= navStickyPos ? 'sticky' : 'mt-5'} {scrollY >= navStickyPos+80 && hideNav ? 'hidden-nav' : ''}">
			<div class="{scrollY >= navStickyPos && innerWidth > 550 ? 'n-label' : 'hidden-n-label'}">
				<span class="n-title"><strong>Test Website</strong></span>
				<img class="n-logo mx-1" src="favicon.png" alt="Rigatini">
			</div>
			<div class="n-buttons {scrollY >= navStickyPos && innerWidth > 550 ? 'n-buttons-shift' : ''} ">
				<button on:click={() => {scrollToSection("about")}}>
					<h3 class="p-2 mb-0">About</h3>
				</button>
				<button on:click={() => {scrollToSection("menu")}}>
					<h3 class="p-2 mb-0">Menu</h3>
				</button>
				<button on:click={() => {scrollToSection("gallery")}}>
					<h3 class="p-2 mb-0">Gallery</h3>
				</button>
				<button on:click={() => {scrollToSection("lower")}}>
					<h3 class="p-2 mb-0">Map</h3>
				</button>
			</div>
		</nav>
	</header>
	<div class="px-3 pb-3 content">
		<div class="section-label">
			
		</div>
		<About aboutData={aboutData}/>
		<h1 class="section-title">Menu</h1>
		<Menu menuData={menuData}/>
		<h1 class="section-title"></h1>
		{#if !loading}
			<Gallery galleryData={galleryData}/>
		{:else}
			<div class="loader mt-4"></div>
		{/if}
		<h1 class="section-title"></h1>
		<Lower />
	</div>
	<footer>
		<div class="footer-container">
			<div>
				<h2 class="f-title"><strong>Test Website</strong></h2>
				<img class="f-logo" src="favicon.png" alt="Rigatini">
			</div>
			<div>
				<div>This restaurant is part of <i>SOULESS</i> of coorporation #168</div>
				<div>All transactions are final, so no takesies backsies</div>
				<div class="f-link"><a href="tel:4692586258">(469) 258-6258</a></div>
				<address class="m-0">
					<div class="f-link"><a target="_blank" and rel="noopener noreferrer" href="https://goo.gl/maps/RU3PW9xjWJwV444F7">1234 SW 1st St, Gainesville, FL</a></div>
				</address>
			</div>
		</div>
	</footer>
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

	footer {
		text-align: left;
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
		transition: all .1s;
	}

	h4 > a:hover{
		color: #81b7d8;
	}

	header > address {
		margin: auto;
	}

	nav {
		display: flex;
		justify-content: space-evenly;
		border-top: 1px solid #F3F3F3;
		z-index: 10;
	}

	.n-buttons {
		display: flex;
		justify-content: space-evenly;
		min-width: 60%;
		transform: translateX(-10%) translateX(-75px);
		transition: .3s all;
	}

	.n-buttons > button {
		background: none;
		border: none;
	}
	.n-buttons > button > h3 {
		color: #F3F3F3;
	}
	.n-buttons > button:hover {
		background: rgba(255, 255, 255, 0.192);
		transition: .1s all;
	}

	.n-buttons-shift {
		transform: none;
		transition: .3s all;
	}

	.n-label {
		position: relative;
		display: flex;
		flex-wrap: nowrap;
		align-items: center;
		transition: .3s all;
	}

	.hidden-n-label{
		position: relative;
		display: flex;
		flex-wrap: nowrap;
		align-items: center;
		transform: translateX(-300px);
		transition: .3s all;
	}

	.n-title{
		font-size:1.5em;
		color: #e6c11d;
		font-family:'Times New Roman', Times, serif;
		white-space: nowrap;
	}
	.n-logo {
		width: 60px;
		height: auto;
	}

	.hidden-nav {
		transform: translateY(-70px);
		transition: .3s all;
	}

	.sticky {
		position: fixed;
		top: 0;
		left: 0;
		width: calc(100%);
		background: black;
		transition: .3s transform;
	}
	.sticky-header-padding {
		padding-bottom: calc(47px + 2.5em);
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

	.footer-container {
		display: flex;
		margin: 0;
		padding: 1em;
		justify-content: space-around;
	}
	.f-title{
		color: #e6c11d;
		font-family:'Times New Roman', Times, serif;
	}
	.f-logo {
		width: 75px;
		height: auto;
	}
	.f-link > a {
		color: #F3F3F3;
		text-decoration: none;
	}
	.f-link > a:hover {
		color: #81b7d8;
		transition: .1s all;
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

	.loader {
        display: inline-block;
        border: 10px solid #f3f3f3; /* Light grey */
        border-top: 10px solid #3498db; /* Blue */
        border-radius: 50%;
        width: 70px;
        height: 70px;
        animation: spin 2s linear infinite;
        transform: translate(0px, 100px);
    }

    @keyframes spin {
        0% { transform: rotate(0deg); }
        100% { transform: rotate(360deg); }
    }
</style>