<script>
	import { onMount } from 'svelte';
	import { Chart, registerables } from 'chart.js';
	import {jwtDecode} from "jwt-decode";

	Chart.register(...registerables);

	let chart = null;
	let data = [];

	let ctx;

	async function fetchWalletData() {
		try {
			const token = localStorage.getItem('token');
			const decodedToken = jwtDecode(token);
			const userId = decodedToken.id;

			const response = await fetch('https://esgi-pa-web-app-back.vercel.app/api/wallet-historic', {
				method: 'post',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({ userId })
			});

			if (!response.ok) {
				throw new Error(`HTTP error! Status: ${response.status}`);
			}

			const apiData = await response.json();
			data = apiData.wallets;
			console.log("data chart =", data)
		} catch (error) {
			console.error('Erreur lors de la récupération des données du portefeuille:', error);
		}
	}

	onMount(async () => {
		await fetchWalletData();
		const labels = data.map((_, index) => `Point ${index + 1}`);

		chart = new Chart(ctx, {
			type: 'line',
			data: {
				labels: labels,
				datasets: [{
					data: data,
					borderColor: 'white',
					borderWidth: 2,
					fill: false,
					tension: 0.1,
					pointRadius: 5,
					pointBackgroundColor: 'white',
				}]
			},
			options: {
				responsive: true,
				maintainAspectRatio: false, // Gérer l'aspect ratio ici
				aspectRatio: 0.8, // Plus bas pour un graphique plus haut
				plugins: {
					legend: {
						display: false
					},
					tooltip: {
						enabled: true
					}
				},
				scales: {
					x: {
						display: true,
						title: {
							display: false, // Retirer le texte "Temps"
						},
						ticks: {
							color: 'white'
						}
					},
					y: {
						display: true,
						title: {
							display: false, // Retirer le texte "Valeur"
						},
						ticks: {
							color: 'white'
						}
					}
				}
			}
		});
	});
</script>

<div class="chart-container">
	<canvas bind:this={ctx}></canvas>
</div>

<style>
	.chart-container {
		width: 80%; /* Réduire la largeur du conteneur */
		margin: 0 auto; /* Centrage horizontal */
		padding: 2rem 5rem;
	}

	canvas {
		background-color: transparent;
		max-width: 95%;
	}
</style>
