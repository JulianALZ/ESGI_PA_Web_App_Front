<script>
	import { onMount } from 'svelte';

	// Valeurs par défaut fictives
	let totalAmount = '624,33 $';
	let changeAmount = '+76,09 $';
	let changePercentage = '13,87%';

	// Simuler un appel API
	async function fetchBalanceData() {
		const response = await new Promise((resolve) => {
			setTimeout(() => {
				resolve({
					totalAmount: '624,33 $',
					changeAmount: '+76,09 $',
					changePercentage: '13,87%'
				});
			}, 1000); // Délai de 1 seconde
		});

		totalAmount = response.totalAmount;
		changeAmount = response.changeAmount;
		changePercentage = response.changePercentage;
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
				padding-top: 5rem;
        padding-bottom: 2rem;
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