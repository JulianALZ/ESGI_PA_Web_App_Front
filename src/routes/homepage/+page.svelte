<script>
	import { onMount } from 'svelte';
	import { jwtDecode } from "jwt-decode";

	import Header from './components/Header.svelte';
	import BalanceSummary from './components/BalanceSummary.svelte';
	import LineChart from './components/LineChart.svelte';
	import ActionButtons from './components/ActionButtons.svelte';
	import CryptoList from './components/CryptoList.svelte';

	onMount(() => {
		const token = localStorage.getItem('token');

		if (!token) {
			window.location.href = '/login';
			return;
		}

		try {
			const decodedToken = jwtDecode(token);
			const currentTime = Date.now() / 1000;
			if (decodedToken.exp < currentTime) {
				window.location.href = '/login';
			}
		} catch (error) {
			console.log(error);
		}
	});
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

	footer {
		background-color: #333;
		color: white;
		padding: 20px 0;
		text-align: center;
		margin-top: 20px;
	}

	.footer-content {
		display: flex;
		justify-content: space-around;
		max-width: 1200px;
		margin: auto;
		padding: 20px;
		text-align: left;
	}

	.footer-section {
		flex: 1;
		padding: 0 10px;
	}

	.footer-section p {
		margin: 5px 0;
	}
</style>

<div class="gradient-custom">
	<Header />
	<BalanceSummary />
	<LineChart />
	<ActionButtons />
	<CryptoList />
	<footer>
		<div class="footer-content">
			<div class="footer-section">
				<p>Projet pour l'ESGI et pour le Projet Annuel 2024</p>
			</div>
			<div class="footer-section">
				<p>&copy; 2024 InvestiQ. Tous droits réservés.</p>
			</div>
			<div class="footer-section">
				<p>Pour nous contacter, envoyer un email à cette adresse:</p>
			</div>
		</div>
	</footer>
</div>
