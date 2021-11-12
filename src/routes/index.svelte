<script>

	// Imports:
	import { onMount } from 'svelte';

	// Initializations:
	let price = 0;
	let supply = 0;
	let treasuryBalance = 0;
	let masterChefBalance = 0;
	let pangolinLiquidity = 0;
	let traderJoeLiquidity = 0;
	let axial = '0xcF8419A615c57511807236751c0AF38Db4ba3351';
	let wavax = '0xb31f66aa3c1e785363f0875a1b74e27b85fd66c7';
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
	const getInfo = (contract, wavaxContract, i) => {
		setTimeout(async () => {
			let traderJoeWAVAX = parseInt(await wavaxContract.balanceOf(traderJoePair)) / wei;
			let pangolinWAVAX = parseInt(await wavaxContract.balanceOf(pangolinPair)) / wei;
			let avaxPrice = (await axios.get('https://api.coingecko.com/api/v3/simple/token_price/avalanche?contract_addresses=' + wavax + '&vs_currencies=usd')).data[wavax].usd;
			traderJoeLiquidity = parseInt(await contract.balanceOf(traderJoePair)) / wei;
			pangolinLiquidity = parseInt(await contract.balanceOf(pangolinPair)) / wei;
			let traderJoePrice = (avaxPrice * traderJoeWAVAX) / traderJoeLiquidity;
			let pangolinPrice = (avaxPrice * pangolinWAVAX) / pangolinLiquidity;
			price = (traderJoePrice + pangolinPrice) / 2;
			supply = parseInt(await contract.totalSupply()) / wei;
			treasuryBalance = parseInt(await contract.balanceOf(treasury)) / wei;
			masterChefBalance = parseInt(await contract.balanceOf(masterChef)) / wei;
			if(i === 999) {
				console.log('Stopping automatic reloading.');
			}
		}, 20000 * i);
	}

	onMount(async () => {

		// Loading Particles:
		particlesJS.load('particles', 'particles.json');

		// Initializing Contracts:
		let contract = new ethers.Contract(axial, minABI, ethers_avax);
		let wavaxContract = new ethers.Contract(wavax, minABI, ethers_avax);

		// Getting AXIAL Info:
		try {
			for(let i = 0; i < 1000; i++) {
				getInfo(contract, wavaxContract, i);
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
		<span>
			<h2>Estimated AXIAL Price:</h2>
			<p>${price.toLocaleString(undefined, {minimumFractionDigits: 2, maximumFractionDigits: 2})}</p>
		</span>
		<span>
			<h2>Total Supply:</h2>
			<p>{supply.toLocaleString(undefined, {minimumFractionDigits: 0, maximumFractionDigits: 0})} AXIAL</p>
		</span>
		<span>
			<h2>Treasury Balance:</h2>
			<p>{treasuryBalance.toLocaleString(undefined, {minimumFractionDigits: 0, maximumFractionDigits: 0})} AXIAL (${(treasuryBalance * price).toLocaleString(undefined, {minimumFractionDigits: 0, maximumFractionDigits: 0})})</p>
		</span>
		<span>
			<h2>Unclaimed Token Rewards:</h2>
			<p>{masterChefBalance.toLocaleString(undefined, {minimumFractionDigits: 0, maximumFractionDigits: 0})} AXIAL (${(masterChefBalance * price).toLocaleString(undefined, {minimumFractionDigits: 0, maximumFractionDigits: 0})})</p>
		</span>
		<span>
			<h2>Trader Joe Liquidity:</h2>
			<p>{traderJoeLiquidity.toLocaleString(undefined, {minimumFractionDigits: 0, maximumFractionDigits: 0})} AXIAL (${(traderJoeLiquidity * price).toLocaleString(undefined, {minimumFractionDigits: 0, maximumFractionDigits: 0})})</p>
		</span>
		<span>
			<h2>Pangolin Liquidity:</h2>
			<p>{pangolinLiquidity.toLocaleString(undefined, {minimumFractionDigits: 0, maximumFractionDigits: 0})} AXIAL (${(pangolinLiquidity * price).toLocaleString(undefined, {minimumFractionDigits: 0, maximumFractionDigits: 0})})</p>
		</span>
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
		justify-content: center;
		align-items: center;
		flex: 1;
	}

	.stats > span {
		display: flex;
		margin: 1.5vh 0;
	}

	.stats > span > h2 {
		margin: 0;
	}

	.stats > span > p {
		font-size: 1.2em;
		margin: .2em .5em;
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
		margin: 3vh 1vw;
	}

</style>