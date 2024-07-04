<script>
	import { loadStripe } from '@stripe/stripe-js';

	let stripePromise = loadStripe('pk_test_51PSnc4P6OFtHWfqTRIA8C5WCF6JmaZtI54b9ZCETjjhS9yQXEKrUmTw1QoyCWN0nDqnVIVbZGkq2EvCJeUOkhcEe00b71upSJB');

	async function handleRetraitClick() {
		const stripe = await stripePromise;
		const { error } = await stripe.redirectToCheckout({
			mode: 'payment',
			lineItems: [{ price: 'price_1Hh1tW2eZvKYlo2CP9zwbDfV', quantity: 1 }],
			successUrl: `${window.location.origin}/success`,
			cancelUrl: `${window.location.origin}/cancel`,
		});
		if (error) console.error('Stripe checkout error', error);
	}

	async function handleDepotClick() {
		window.location.href = 'https://donate.stripe.com/test_eVa9E6gc75xx7PG7ss';
	}
</script>

<style>
	.buttons-container {
		display: flex;
		justify-content: center; /* Centré pour un meilleur aspect visuel */
		padding: 2rem; /* Ajuster le padding pour éviter que les boutons ne débordent */
		width: 100%;
		gap: 2rem; /* Espacement entre les boutons */
		flex-wrap: wrap; /* Permet aux boutons de passer à la ligne si nécessaire */
	}

	.button {
		margin: 1rem; /* Espacement entre les boutons et leurs marges */
		padding: 1rem 2rem; /* Réduction de la largeur du padding */
		font-size: 16px;
		font-weight: bold;
		color: white;
		background: linear-gradient(145deg, #3a3a3a, #2c2c2c); /* Couleur légèrement plus claire */
		border: none;
		border-radius: 30px;
		cursor: pointer;
		transition: background-color 0.3s, transform 0.2s, box-shadow 0.3s;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.button::after {
		content: attr(data-symbol);
		font-size: 24px;
		background: #ff4500;
		color: white;
		width: 40px;
		height: 40px;
		border-radius: 50%;
		display: flex;
		justify-content: center;
		align-items: center;
		transition: transform 0.3s;
	}

	.button:hover {
		background-color: #4a4a4a;
		box-shadow: 0 4px 8px rgba(0,0,0,0.2);
	}

	.button:hover::after {
		transform: scale(1.1);
	}

	.button-retrait::after {
		content: '-';
	}

	.button-depot::after {
		content: '+';
	}

	.button-text {
		font-size: 1.6rem;
		padding-right: 2rem;
	}

	@media (max-width: 768px) {
		.button {
			padding: 1rem 1.5rem;
			font-size: 14px;
		}

		.button::after {
			width: 30px;
			height: 30px;
			font-size: 20px;
		}

		.button-text {
			padding-right: 1rem;
		}
	}
</style>

<div class="buttons-container">
	<button class="button button-retrait" on:click={handleRetraitClick} data-symbol="-">
		<span class="button-text">Retrait</span>
	</button>
	<button class="button button-depot" on:click={handleDepotClick} data-symbol="+">
		<span class="button-text">Dépôt</span>
	</button>
</div>
