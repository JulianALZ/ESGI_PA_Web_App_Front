<script>
	import { onMount } from 'svelte';
	import { Chart, registerables } from 'chart.js';

	Chart.register(...registerables);

	let chart = null;
	let data = [];

	let ctx;

	// Appel API pour récupérer les données du portefeuille
	async function fetchWalletData() {
		try {
			const response = await fetch(`https://esgi-pa-web-app-back.vercel.app/api/wallet-historic`);
			if (!response.ok) {
				throw new Error(`HTTP error! Status: ${response.status}`);
			}
			const apiData = await response.json();
			data = apiData.wallets;
		} catch (error) {
			console.error('Erreur lors de la récupération des données du portefeuille:', error);
		}
	}

	onMount(async () => {
		await fetchWalletData();
		chart = new Chart(ctx, {
			type: 'line',
			data: {
				labels: new Array(data.length).fill(''), // Créer un tableau vide pour les labels
				datasets: [{
					data: data,
					borderColor: 'white',
					borderWidth: 2,
					fill: false,
					tension: 0.1,
					pointRadius: 0 // Pas de points sur la ligne
				}]
			},
			options: {
				responsive: true, // Activer la responsivité
				maintainAspectRatio: false,
				plugins: {
					legend: {
						display: false // Pas de légende
					},
					tooltip: {
						enabled: false // Pas de tooltips
					}
				},
				scales: {
					x: { // Notez que c'est 'x' au lieu de 'xAxes'
						display: false
					},
					y: { // Notez que c'est 'y' au lieu de 'yAxes'
						display: false
					}
				}
			}
		});
	});
</script>

<div class="chart-container" style="width: 90%;">
	<canvas bind:this={ctx}></canvas>
</div>

<style>
    .chart-container {
        margin: 0 auto; /* Pour centrer horizontalement */
        text-align: center; /* Pour centrer le canvas dans la div */
        padding-bottom: 10rem;
		padding-top: 10rem;
    }

    canvas {
        background-color: transparent; /* Le canvas sera transparent */
    }
</style>

