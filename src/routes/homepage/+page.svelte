<script>
	import { onMount } from 'svelte';

	onMount(() => {
		const token = localStorage.getItem('token');

		if (!token) {
			window.location.href = '/login';
			return;
		}

		try {
			const decodedToken = jwt_decode(token);
			const currentTime = Date.now() / 1000;
			if(decodedToken.exp < currentTime){
				window.location.href = '/login';
			}
		} catch (error) {
			console.log(error);
		}
	});


	// Importez tous les composants ici
	import Header from './components/Header.svelte';
	import BalanceSummary from './components/BalanceSummary.svelte';
	import LineChart from './components/LineChart.svelte';
	import ActionButtons from './components/ActionButtons.svelte';
	import CryptoList from './components/CryptoList.svelte';

</script>

<style>
	.gradient-custom {
    /* fallback for old browsers */
    background: #6a11cb;

    /* Chrome 10-25, Safari 5.1-6 */
    background: -webkit-linear-gradient(to right, rgba(106, 17, 203, 1), rgba(37, 117, 252, 1));

    /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
    background: linear-gradient(to right, rgba(106, 17, 203, 1), rgba(37, 117, 252, 1));

    /* Assurez-vous que le gradient couvre toute la hauteur et la largeur de la page */
    min-height: 100vh;
    width: 100%;
}
</style>

<div class="gradient-custom">
	<Header />
	<BalanceSummary />
	<LineChart />
	<ActionButtons />
	<CryptoList />
</div>




