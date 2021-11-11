<script>

	// Imports:
	import { onMount } from 'svelte';

	// Initializations:
	let supply = 0;
	let treasuryBalance = 0;
	let masterChefBalance = 0;
	let pangolinLiquidity = 0;
	let traderJoeLiquidity = 0;
	let axial = '0xcF8419A615c57511807236751c0AF38Db4ba3351';
	let treasury = '0x4980ad7ccb304f7d3c5053aa1131ed1edaf48809';
	let masterChef = '0x958c0d0baa8f220846d3966742d4fb5edc5493d3';
	let traderJoePair = '0x5305A6c4DA88391F4A9045bF2ED57F4BF0cF4f62';
	let pangolinPair = '0x53d4bF164c53547b4278A0352C292162C154AfE5';
	let minABI = [
    { constant: true, inputs: [{ name: "", type: "address" }], name: "balanceOf", outputs: [{ name: "", type: "uint256" }], type: "function" },
    { constant: true, inputs: [], name: "totalSupply", outputs: [{ name: "", type: "uint256" }], type: "function" }
  ];
	let wei = 10 ** 18;

	// Function to get AXIAL Info:
	const getInfo = (contract, i) => {
		setTimeout(async () => {
			supply = parseInt(await contract.totalSupply()) / wei;
			treasuryBalance = parseInt(await contract.balanceOf(treasury)) / wei;
			masterChefBalance = parseInt(await contract.balanceOf(masterChef)) / wei;
			traderJoeLiquidity = parseInt(await contract.balanceOf(traderJoePair)) / wei;
			pangolinLiquidity = parseInt(await contract.balanceOf(pangolinPair)) / wei;
			if(i === 999) {
				console.log('Stopping automatic reloading.');
			}
		}, 20000 * i);
	}

	onMount(async () => {

		// Loading Particles:
		particlesJS.load('particles', 'particles.json');

		// Initializing AXIAL Contract:
		let contract = new ethers.Contract(axial, minABI, ethers_avax);

		// Getting AXIAL Info:
		try {
			for(let i = 0; i < 1000; i++) {
				getInfo(contract, i);
			}
		} catch {
			console.log('Stopping automatic reloading.');
		}

	});

</script>

<!-- #################################################################################################### -->

<!-- Dynamic Svelte Header -->
<svelte:head>
	<title>Axial Launch</title>
	<meta name="description" content="Quick app to track the launch of Axial's new token.">
</svelte:head>

<!-- Particles Background -->
<div id="particles"></div>

<!-- Page Content -->
<div class="content">

	<!-- Page Banner -->
	<div class="banner">
		<img src="banner.png" alt="Axial Banner">
	</div>

	<!-- AXIAL Token Stats -->
	<div class="stats">
		<p><strong>Total Supply:</strong> {supply.toLocaleString(undefined, {minimumFractionDigits: 0, maximumFractionDigits: 0})} AXIAL</p>
		<p><strong>Treasury Balance:</strong> {treasuryBalance.toLocaleString(undefined, {minimumFractionDigits: 0, maximumFractionDigits: 0})} AXIAL</p>
		<p><strong>Unclaimed Token Rewards:</strong> {masterChefBalance.toLocaleString(undefined, {minimumFractionDigits: 0, maximumFractionDigits: 0})} AXIAL</p>
		<p><strong>Trader Joe Liquidity:</strong> {traderJoeLiquidity.toLocaleString(undefined, {minimumFractionDigits: 0, maximumFractionDigits: 0})} AXIAL</p>
		<p><strong>Pangolin Liquidity:</strong> {pangolinLiquidity.toLocaleString(undefined, {minimumFractionDigits: 0, maximumFractionDigits: 0})} AXIAL</p>
	</div>

	<!-- Links -->
	<div class="links">
		<a href="https://axial.exchange">Home</a>
		<p>|</p>
		<a href="https://app.axial.exchange">App</a>
		<p>|</p>
		<a href="https://docs.axial.exchange">Docs</a>
	</div>
</div>

<!-- #################################################################################################### -->

<style>

	#particles {
		position: fixed;
		height: 100%;
		width: 100%;
		background-color: #201F55;
		overflow: hidden;
		z-index: -1;
	}

	.content {
		height: 100vh;
		display: flex;
		flex-direction: column;
		color: #EAAB50;
		overflow: hidden;
	}

	.banner {
		display: flex;
		flex: 0 1;
		border-bottom: 4px solid #EAAB50;
	}

	.banner > img {
		width: 100vw;
	}

	.stats {
		display: flex;
		flex-direction: column;
		flex: 1;
		justify-content: center;
		align-items: center;
		font-size: 1.5em;
	}

	.stats > p {
		margin: 3vh 0;
	}

	.links {
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.links > a {
		color: #EAAB50;
		text-decoration: none;
	}

	.links > p {
		margin-inline: 1vw;
	}

</style>