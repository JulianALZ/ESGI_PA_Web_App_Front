<script>
	import { loadStripe } from '@stripe/stripe-js';
	import { jwtDecode } from "jwt-decode";
	import { onMount } from 'svelte';

	let stripePromise = loadStripe('pk_test_51PSnc4P6OFtHWfqTRIA8C5WCF6JmaZtI54b9ZCETjjhS9yQXEKrUmTw1QoyCWN0nDqnVIVbZGkq2EvCJeUOkhcEe00b71upSJB');
	let userId = 1;
	let showModal = false;
	let amount = '';
	let message = '';
	let isLoading = false;

	onMount(() => {
		const token = localStorage.getItem('token');
		if (token) {
			const decodedToken = jwtDecode(token);
			userId = decodedToken.id;
		}
	});

	async function handleRetraitClick() {
		console.log('Retrait button clicked');
		showModal = true;
		console.log('showModal set to true');
	}

	async function handleSubmitRetrait() {
		if (!amount) return;

		isLoading = true;
		try {
			const response = await fetch('https://esgi-pa-web-app-back.vercel.app/api/retrait', {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({ userId, amount: parseFloat(amount) })
			});
			const data = await response.json();
			message = data.message;

			if (response.ok) {
				setTimeout(() => {
					location.reload();
				}, 2000); // Actualiser la page après 2 secondes
			}
		} catch (error) {
			message = 'Erreur lors de la transaction';
		} finally {
			isLoading = false;
		}
	}

	function handleCloseModal() {
		console.log('Closing modal');
		showModal = false;
		amount = '';
		message = '';
	}

	function handleKeyDown(event) {
		if (event.key === 'Enter' || event.key === ' ') {
			handleCloseModal();
		}
	}

	async function handleDepotClick() {
		const token = localStorage.getItem('token');
		const decodedToken = jwtDecode(token);
		const userId = decodedToken.id;
		window.location.href = 'https://donate.stripe.com/test_eVa9E6gc75xx7PG7ss?client_reference_id=' + userId;
	}
</script>

<style>
	/* Vos styles existants */
	.modal {
		position: fixed;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		background-color: #2C2F33;
		padding: 1rem;
		box-shadow: 0 4px 8px rgba(0,0,0,0.2);
		border-radius: 8px;
		z-index: 1000;
		width: 80%;
		max-width: 400px;
		display: flex;
		flex-direction: column;
		align-items: center;
		max-height: 25vh;
		overflow-y: auto;
	}

	.modal h2 {
		margin-bottom: 1rem;
		font-size: 1.5rem;
		color: white;
	}

	.modal input {
		margin-bottom: 1rem;
		padding: 0.5rem;
		width: 100%;
		border: 1px solid #ccc;
		border-radius: 4px;
	}

	.modal button {
		margin: 0.8rem;
		padding: 0.8rem 3rem;
		background-color: #007bff;
		border: none;
		border-radius: 15px;
		color: white;
		cursor: pointer;
		background: linear-gradient(to right, rgba(106, 17, 203, 1), rgba(37, 117, 252, 1));
	}

	.modal button:disabled {
		background-color: #ccc;
		cursor: not-allowed;
	}

	.modal p {
		margin-top: 1rem;
		color: white;
		text-align: center;
	}

	.modal-backdrop {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: rgba(0,0,0,0.5);
		z-index: 500;
	}

	.buttons-container {
		display: flex;
		justify-content: center;
		padding: 2rem;
		width: 100%;
		gap: 2rem;
		flex-wrap: wrap;
	}

	.button {
		margin: 1rem;
		padding: 1rem 2rem;
		font-size: 16px;
		font-weight: bold;
		color: white;
		background: linear-gradient(145deg, #3a3a3a, #2c2c2c);
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

{#if showModal}
	<div class="modal-backdrop" on:click={handleCloseModal} on:keydown={handleKeyDown} tabindex="0" role="button"></div>
	<div class="modal" role="dialog">
		<h2>Retrait</h2>
		{#if message === ''}
			<input type="number" bind:value={amount} placeholder="Entrez le montant" />
			<div>
				<button on:click={handleSubmitRetrait} disabled={isLoading}>Valider</button>
				<button on:click={handleCloseModal} disabled={isLoading}>Annuler</button>
			</div>
		{:else}
			<p>{message}</p>
			<button on:click={handleCloseModal}>Fermer</button>
		{/if}
	</div>
{/if}
