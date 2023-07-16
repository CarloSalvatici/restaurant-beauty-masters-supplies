<script>
    export let galleryData

	let enlargeBool = false
	let enlargedImage
	const enlargeImage = (imageData) => {
		enlargedImage = {
			URL: imageData.URL,
			name: imageData.name
		}
		enlargeBool = true
	}
</script>

<main>
    <div id="lower" class="lower">
		<!--Main loop for showing the list of images-->
        <iframe title="Restaurant Location" src="https://www.google.com/maps/embed?pb=!1m14!1m12!1m3!1d13876.026461110398!2d-82.356038!3d29.603493949999997!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!5e0!3m2!1sen!2sus!4v1689519842942!5m2!1sen!2sus" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
    </div>
</main>

<style>
    .lower > iframe {
        width: 100%;
		height: 75vh;
    }
</style>