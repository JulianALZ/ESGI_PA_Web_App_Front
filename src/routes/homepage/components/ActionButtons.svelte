<script>
	import { loadStripe } from '@stripe/stripe-js';

	let stripePromise;

	const getStripe = () => {
		if (!stripePromise) {
			stripePromise = loadStripe('pk_test_51PSnc4P6OFtHWfqTRIA8C5WCF6JmaZtI54b9ZCETjjhS9yQXEKrUmTw1QoyCWN0nDqnVIVbZGkq2EvCJeUOkhcEe00b71upSJB'); // Remplacez par votre clé publique Stripe
		}
		return stripePromise;
	};

	export const redirectToCheckout = async (userId) => {
		const stripe = await getStripe();

		const { error } = await stripe.redirectToCheckout({
			mode: 'payment',
			lineItems: [
				{
					price: 'price_1Hh1tW2eZvKYlo2CP9zwbDfV', // Remplacez par l'ID de votre prix Stripe
					quantity: 1
				}
			],
			successUrl: window.location.origin + '/success',
			cancelUrl: window.location.origin + '/cancel',
			clientReferenceId: userId // Assurez-vous de transmettre l'ID de l'utilisateur
		});

		if (error) {
			console.error('Stripe checkout error', error);
		}
	};

	let userId = 1; // Remplacez par la méthode pour obtenir l'ID de l'utilisateur connecté

	function handleRetraitClick() {
		console.log('Retrait clicked');
	}

	function handleDepotClick() {
		console.log('Dépôt clicked');
		window.location.href = 'https://donate.stripe.com/test_eVa9E6gc75xx7PG7ss'; // Remplacez par votre lien de paiement Stripe
	}
</script>

<style>
	.button {
		color: white;
		font-weight: 600;
		padding: 1rem 4rem;
		border-radius: 9999px;
		font-size: 1.25rem;
		border: none;
		display: block;
		width: 100%;
		margin-bottom: 0.5rem;
	}

	.button-retrait {
		background-color: #6c757d;
	}

	.button-depot {
		background-color: #ffc107;
	}

	.container-custom {
		background-color: #12343b;
		padding: 1rem;
		display: flex;
		flex-direction: column;
		align-items: center;
		padding-top: 3rem;
		padding-bottom: 3rem;
	}

	@media (min-width: 768px) {
		.container-custom {
			flex-direction: row;
			justify-content: space-around;
		}
		.button {
			display: inline-block;
			width: auto;
			margin-bottom: 0;
		}
	}
</style>

<div class="container-custom mt-5">
	<button
			class="button button-retrait"
			on:click={handleRetraitClick}
	>
		Retrait
	</button>
	<button
			class="button button-depot"
			on:click={handleDepotClick}
	>
		Dépôt
	</button>
</div>




<!--<script>-->
<!--	function handleRetraitClick() {-->
<!--		console.log('Retrait clicked');-->
<!--	}-->

<!--	function handleDepotClick() {-->
<!--		console.log('Dépôt clicked');-->
<!--		window.location.href = 'https://donate.stripe.com/test_eVa9E6gc75xx7PG7ss';-->
<!--	}-->
<!--</script>-->

<!--<style>-->
<!--    .button {-->
<!--        color: white;-->
<!--        font-weight: 600; /* font-semibold */-->
<!--        padding: 1rem 4rem; /* padding plus grand pour des boutons plus gros */-->
<!--        border-radius: 9999px; /* rounded-full */-->
<!--        font-size: 1.25rem; /* taille de police plus grande pour de meilleurs visuels */-->
<!--        border: none; /* pas de bordure */-->
<!--        display: block; /* s'assurer que les boutons s'étendent pleinement */-->
<!--        width: 100%; /* pleine largeur pour la responsivité */-->
<!--        margin-bottom: 0.5rem; /* espace entre les boutons sur les petits écrans */-->
<!--    }-->

<!--    .button-retrait {-->
<!--        background-color: #6c757d; /* Gris foncé pour le bouton Retrait */-->
<!--    }-->

<!--    .button-depot {-->
<!--        background-color: #ffc107; /* Jaune pour le bouton Dépôt */-->
<!--    }-->

<!--    .container-custom {-->
<!--        background-color: #12343b; /* Gris clair pour le fond */-->
<!--        padding: 1rem; /* py-4 ajusté pour tous les côtés */-->
<!--        display: flex;-->
<!--        flex-direction: column; /* empile les boutons verticalement sur les petits écrans */-->
<!--        align-items: center; /* centre les boutons */-->
<!--        padding-top: 3rem;-->
<!--        padding-bottom: 3rem;-->
<!--    }-->

<!--    @media (min-width: 768px) {-->
<!--        /* Sur les écrans plus larges, affiche les boutons côte à côte */-->
<!--        .container-custom {-->
<!--            flex-direction: row; /* retourne à une disposition horizontale */-->
<!--            justify-content: space-around; /* espace entre les boutons */-->
<!--        }-->
<!--        .button {-->
<!--            display: inline-block; /* boutons en ligne */-->
<!--            width: auto; /* annule la pleine largeur */-->
<!--            margin-bottom: 0; /* retire l'espace supplémentaire en bas */-->
<!--        }-->
<!--    }-->
<!--</style>-->

<!--<div class="container-custom mt-5">-->
<!--	<button-->
<!--		class="button button-retrait"-->
<!--		on:click={handleRetraitClick}-->
<!--	>-->
<!--		Retrait-->
<!--	</button>-->
<!--	<button-->
<!--		class="button button-depot"-->
<!--		on:click={handleDepotClick}-->
<!--	>-->
<!--		Dépôt-->
<!--	</button>-->
<!--</div>-->
