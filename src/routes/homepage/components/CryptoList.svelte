<script>
	import { onMount } from 'svelte';
	import { Chart } from 'chart.js';

	let chart;
	let chartData = {
		labels: [],
		datasets: [
			{
				data: [],
				backgroundColor: []
			}
		]
	};

	// Fonction pour générer des couleurs dynamiques
	function generateColors(count) {
		const colors = [];
		for (let i = 0; i < count; i++) {
			colors.push(`hsl(${(i * 360 / count)}, 70%, 50%)`);
		}
		return colors;
	}

	// Simuler des données dynamiques
	function fetchData() {
		// Simule une réponse d'API
		return {
			labels: ['Red', 'Blue', 'Yellow', 'Green', 'Purple', 'Orange'],
			data: [12, 19, 3, 5, 2, 3]
		};
	}

	function updateChartData() {
		const response = fetchData();
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
						display: false // We handle legend ourselves
					}
				}
			}
		});

		updateChartData();
	});

	function getPercentage(value, total) {
		return ((value / total) * 100).toFixed(2);
	}

	function getTotal(data) {
		return data.reduce((acc, value) => acc + value, 0);
	}

	let total;
	$: total = getTotal(chartData.datasets[0].data);
</script>

<style>
	body {
		margin: 0; /* Enlève les marges par défaut pour supprimer les barres blanches */
	}

	.chart-container {
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: #12343b;
		padding: 3rem;
		min-height: 100vh;
		box-sizing: border-box; /* Inclut padding et border dans les dimensions de l'élément */
		flex-direction: column; /* Par défaut, affiche la légende sous le graphique */
	}

	.chart-section {
		display: flex;
		align-items: center;
		justify-content: center;
		padding-right: 0; /* Initialement sans padding droit */
	}

	.legend-section {
		padding-top: 2rem;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	#myPieChart {
		width: 250px; /* Taille du graphique ajustée pour les petits écrans */
		height: 250px; /* Maintient le rapport d'aspect */
	}

	.data-list {
		list-style-type: none;
		padding: 0;
		margin-top: 0;
	}

	.data-list li {
		margin: 10px 0;
		display: flex;
		align-items: center;
		justify-content: left; /* Aligne les éléments à gauche pour une meilleure lisibilité */
		font-size: 1rem; /* Taille de police appropriée pour la lisibilité */
		color: white; /* Texte blanc pour contraster avec le fond sombre */
		padding-left: 1rem; /* Ajoute un padding à gauche pour décaler les éléments */
	}

	.color-box {
		width: 20px;
		height: 20px;
		display: inline-block;
		margin-right: 10px;
		border-radius: 3px; /* Ajoute un léger arrondi aux couleurs */
	}

	@media (min-width: 768px) {
		.chart-container {
			flex-direction: row; /* Affiche la légende à droite du graphique sur les écrans moyens et grands */
		}
		.chart-section {
			flex: 1;
			justify-content: flex-end;
			padding-right: 2rem; /* Ajoute de l'espace entre le graphique et la légende */
		}
		.legend-section {
			flex: 1;
			padding-left: 2rem;
			padding-top: 0;
			align-items: center; /* Centre les éléments de la légende */
		}
		#myPieChart {
			width: 300px; /* Augmente la taille du graphique pour les écrans moyens */
			height: 300px;
		}
	}

	@media (min-width: 1200px) {
		.legend-section {
			padding-left: 6rem; /* Augmente le padding left sur les écrans très larges */
		}
		#myPieChart {
			width: 350px; /* Augmente la taille du graphique pour les écrans très larges */
			height: 350px;
		}
	}
</style>

<div class="chart-container">
	<div class="chart-section">
		<canvas id="myPieChart"></canvas>
	</div>
	<div class="legend-section">
		<ul class="data-list">
			{#each chartData.labels as label, index}
				<li>
					<div class="color-box" style="background-color: {chartData.datasets[0].backgroundColor[index]};"></div>
					<span>{label}: {chartData.datasets[0].data[index]} ({getPercentage(chartData.datasets[0].data[index], total)}%)</span>
				</li>
			{/each}
		</ul>
	</div>
</div>
