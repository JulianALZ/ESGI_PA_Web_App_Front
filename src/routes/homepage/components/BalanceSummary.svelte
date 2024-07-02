<script>
	import { onMount } from 'svelte';

	// Valeurs par défaut fictives
	let totalAmount = '0,00 $';
	let changeAmount = '0,00 $';
	let changePercentage = '0,00%';

	// Appel API pour récupérer les données de balance
	async function fetchBalanceData() {
		try {
			const response = await fetch(`https://esgi-pa-web-app-back.vercel.app/api/wallet-historic`);
			if (!response.ok) {
				throw new Error(`HTTP error! Status: ${response.status}`);
			}
			const data = await response.json();
			console.log(data)
			totalAmount = `${data.lastWallet} $`;
			changeAmount = `${data.walletChange} $`;
			changePercentage = `${data.percentageChange}%`;
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
	<p> + {changeAmount} </p>
</div>
<!--<div class="balance-summary ">-->
<!--	<h2>{totalAmount}</h2>-->
<!--	<p>{changeAmount} ({changePercentage})</p>-->
<!--</div>-->

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