<script>
	import { onMount } from 'svelte';
	import { Chart, registerables } from 'chart.js';

	Chart.register(...registerables);

	let chart = null;

	// Données fictives pour le graphique
	let data = [65, 59, 80, 81, 56, 55, 40, 70, 85, 50, 45, 60];

	let ctx;

	onMount(() => {
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
        padding-bottom: 3rem;
    }

    canvas {
        background-color: transparent; /* Le canvas sera transparent */
    }
</style>



