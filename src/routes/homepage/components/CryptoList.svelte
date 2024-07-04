<script>
	import { onMount } from 'svelte';
	import { Chart } from 'chart.js';

	let chart;
	let chartData = {
		labels: [],
		datasets: [{
			data: [],
			backgroundColor: []
		}]
	};

	// Fonction pour générer des couleurs dynamiques
	function generateColors(count) {
		const colors = [];
		for (let i = 0; i < count; i++) {
			colors.push(`hsl(${i * 360 / count}, 70%, 50%)`);
		}
		return colors;
	}

	// Fonction pour récupérer les données depuis l'API
	async function fetchData() {
		try {
			const response = await fetch('https://esgi-pa-web-app-back.vercel.app/api/positions');
			if (!response.ok) {
				throw new Error(`HTTP error! Status: ${response.status}`);
			}
			const data = await response.json();
			return {
				labels: data.symbols,
				data: data.percentages
			};
		} catch (error) {
			console.error('Error fetching positions:', error);
			return {
				labels: [],
				data: []
			};
		}
	}

	async function updateChartData() {
		const response = await fetchData();
		chartData.labels = response.labels;
		chartData.datasets[0].data = response.data;
		chartData.datasets[0].backgroundColor = generateColors(response.data.length);
		if (chart) {
			chart.update();
		}
	}

	onMount(() => {
		const ctx = document.getElementById('myPieChart').getContext('2d');
		chart = new Chart(ctx, {
			type: 'pie',
			data: chartData,
			options: {
				responsive: true,
				plugins: {
					legend: {
						display: true, // Affichage de la légende
						position: 'bottom',
						labels: {
							generateLabels: function(chart) {
								const data = chart.data;
								if (data.labels.length && data.datasets.length) {
									return data.labels.map((label, i) => {
										const value = data.datasets[0].data[i];
										const total = data.datasets[0].data.reduce((acc, val) => acc + val, 0);
										const percentage = ((value / total) * 100).toFixed(1);
										return {
											text: `${label}: ${percentage}%`,
											fillStyle: data.datasets[0].backgroundColor[i],
											hidden: false,
											lineCap: 'butt',
											lineDash: [],
											lineDashOffset: 0,
											lineJoin: 'miter',
											strokeStyle: data.datasets[0].borderColor ? data.datasets[0].borderColor[i] : 'rgba(0,0,0,0)',
											pointStyle: 'circle',
											rotation: 0,
											textAlign: 'left',
											fontColor: '#FFFFFF' // Couleur de la légende
										};
									});
								}
								return [];
							},
							padding: 30,
							color: '#FFFFFF' // Couleur de la légende
						}
					}
				},
				maintainAspectRatio: false
			}
		});

		updateChartData();
	});
</script>

<style>
	.chart-card {
		width: 80%;
		max-width: 1200px;
		margin: 4rem auto; /* Espacement relatif pour rendre la carte responsive */
		padding: 2rem;
		background-color: #2C2F33;
		border-radius: 10px;
		box-shadow: 0 4px 8px rgba(0,0,0,0.2);
	}

	.chart-header {
		text-align: center;
		font-size: 1.5rem;
		color: white;
		margin-bottom: 2rem;
	}

	.chart-container {
		position: relative;
		height: 60vh; /* Utiliser des unités relatives pour la hauteur */
	}

	canvas {
		display: block;
		max-width: 100%;
	}
</style>

<div class="chart-card">
	<div class="chart-header">Répartitions des possitions</div>
	<div class="chart-container">
		<canvas id="myPieChart"></canvas>
	</div>
</div>
