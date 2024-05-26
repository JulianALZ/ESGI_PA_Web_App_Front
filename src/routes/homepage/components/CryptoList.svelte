<script>
	import { onMount } from 'svelte';

	let cryptoList = [];

	onMount(async () => {
		// Ici, vous effectuerez votre appel API pour remplir cryptoList
		cryptoList = [
			{
				name: 'Bitcoin',
				amount: 0.5,
				value: 15886.5,
				change: -2.5,
				valueInUsd: 7500,
				logo: '/path/to/bitcoin-logo.png',
				symbol: 'BTC'
			},
			{
				name: 'Ethereum',
				amount: 2,
				value: 3000,
				change: 5.1,
				valueInUsd: 6000,
				logo: '/path/to/ethereum-logo.png',
				symbol: 'ETH'
			},
			{
				name: 'Ethereum',
				amount: 2,
				value: 3000,
				change: 5.1,
				valueInUsd: 6000,
				logo: '/path/to/ethereum-logo.png',
				symbol: 'ETH'
			},
			{
				name: 'Ethereum',
				amount: 2,
				value: 3000,
				change: 5.1,
				valueInUsd: 6000,
				logo: '/path/to/ethereum-logo.png',
				symbol: 'ETH'
			},
			// Ajoutez plus d'objets ici pour d'autres cryptomonnaies.
		];
	});
</script>

<style>
    .crypto-container {
        background-color: #1e1e1e; /* Nouvelle couleur de fond */
        color: white;
        padding: 12px 24px; /* Ajustement de l'espacement interne */
        margin: 8px 0; /* Espacement entre les éléments de la liste */
        display: flex;
        align-items: center;
        justify-content: space-between; /* Alignement des éléments */
        border-radius: 8px;
    }

    .crypto-logo {
        height: 40px;
        width: 40px;
        border-radius: 50%;
        object-fit: cover;
        margin-right: 16px; /* Ajustement de la marge droite */
    }

    .crypto-details {
        flex: 1; /* Donne de l'espace supplémentaire autour des détails */
    }

    .crypto-name {
        font-weight: bold;
    }

    .crypto-change-positive {
        color: #26a17b; /* Vert pour la valeur en hausse */
    }

    .crypto-change-negative {
        color: #e15241; /* Rouge pour la valeur en baisse */
    }

    .crypto-value {
        text-align: right;
        flex: 1; /* Donne de l'espace supplémentaire autour des valeurs */
    }

    .container-custom {
        background-color: #12343b;
        padding-top: 3rem;
        padding-bottom: 3rem;
				padding-left: 3rem;
				padding-right: 3rem;

    }
</style>

<div class="container-custom">
	{#each cryptoList as crypto}
		<div class="crypto-container">
			<img src={crypto.logo} alt={crypto.name} class="crypto-logo" />
			<div class="crypto-details">
				<div class="crypto-name">{crypto.name}</div>
				<div class="text-sm">{crypto.amount} {crypto.symbol} - ${crypto.value.toLocaleString('en-US')}</div>
			</div>
			<div class="crypto-value">
				<div>${crypto.valueInUsd.toLocaleString('en-US')}</div>
				<div class={crypto.change < 0 ? 'crypto-change-negative' : 'crypto-change-positive'}>
					{crypto.change}%
				</div>
			</div>
		</div>
	{/each}
</div>
