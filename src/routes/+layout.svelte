<script>
	import '../app.pcss';
</script>

<svelte:head>
	<title>Gian & Laiza's Wedding</title>
	<meta name="description" content="You're invited to Gian and Laiza's wedding." />
	<meta name="keywords" content="wedding,invitation,gian,laiza" />
	<meta name="author" content="Carlo" />

	<!-- Open Graph / Facebook -->
	<meta property="og:site_name" content="Gian & Laiza's Wedding" />
	<meta property="og:locale" content="en" />
	<meta property="og:type" content="website" />
	<!-- <meta property="og:url" content="URL of your website" /> -->
	<meta property="og:title" content="Gian & Laiza's Wedding" />
	<meta property="og:description" content="You're invited to Gian and Laiza's wedding." />
	<meta property="og:image" content="/ogimage.png" />
	<meta property="og:image:width" content="1200" />
	<meta property="og:image:height" content="630" />

	<!-- Twitter -->
	<meta property="twitter:card" content="summary_large_image" />
	<meta property="twitter:url" content="https://gianlaiza-wedding.netlify.app" />
	<meta property="twitter:title" content="Gian & Laiza's Wedding" />
	<meta property="twitter:description" content="You're invited to Gian and Laiza's wedding." />
	<meta property="twitter:image" content="/ogimage.png" />

	<!-- Favicon -->
	<!-- <link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png" />
	<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png" />
	<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png" />
	<link rel="manifest" href="/site.webmanifest" /> -->
	<!-- <link rel="mask-icon" href="/safari-pinned-tab.svg" color="#5bbad5" /> -->
	<meta name="msapplication-TileColor" content="#ffffff" />
	<meta name="theme-color" content="#ffffff" />
</svelte:head>

<slot />
