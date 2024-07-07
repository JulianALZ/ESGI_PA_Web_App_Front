<script>
	import { onMount } from 'svelte';
	import {jwtDecode} from "jwt-decode";

	// Valeurs par défaut fictives
	let totalAmount = '0,00 $';
	let changeAmount = '0,00 $';
	let changePercentage = '0,00%';

	// Appel API pour récupérer les données de balance
	async function fetchBalanceData() {
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

			const data = await response.json();
			console.log("wallet-historic balance = ", data)

			totalAmount = `${Number(data.lastWallet).toFixed(2)} $`;
			changeAmount = `${Number(data.changeAmount).toFixed(2)} $`;
			changePercentage = data.percentageChange !== null && data.percentageChange !== undefined
					? `${Number(data.percentageChange).toFixed(2)}%`
					: 'N/A';

		} catch (error) {
			console.error('Erreur lors de la récupération des données de balance:', error);
		}
	}

	// Appeler fetchBalanceData() après que le composant soit monté
	onMount(() => {
		fetchBalanceData();
	});
</script>

<div class="balance-summary ">
	<h2>{totalAmount}</h2>
	<p>{changeAmount} ({changePercentage})</p>
</div>

<style>
    .balance-summary {
        text-align: center;
        color: white;
        padding: 1rem;
		padding-top: 10rem;
        padding-bottom: 5rem;
        /* Remplacez les valeurs ci-dessous avec votre dégradé personnalisé ou couleur */
    }

    .balance-summary h2 {
        font-size: 2.5rem; /* Ajustez la taille selon votre design */
        margin-bottom: 0.5rem;
    }

    .balance-summary p {
        font-size: 1rem; /* Ajustez la taille selon votre design */
        margin-bottom: 0; /* Supprimez la marge inférieure par défaut du paragraphe */
    }
</style>