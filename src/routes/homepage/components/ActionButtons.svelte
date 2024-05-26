<script>
	function handleRetraitClick() {
		console.log('Retrait clicked');
	}

	function handleDepotClick() {
		console.log('Dépôt clicked');
	}
</script>

<style>
    .button {
        color: white;
        font-weight: 600; /* font-semibold */
        padding: 1rem 4rem; /* padding plus grand pour des boutons plus gros */
        border-radius: 9999px; /* rounded-full */
        font-size: 1.25rem; /* taille de police plus grande pour de meilleurs visuels */
        border: none; /* pas de bordure */
        display: block; /* s'assurer que les boutons s'étendent pleinement */
        width: 100%; /* pleine largeur pour la responsivité */
        margin-bottom: 0.5rem; /* espace entre les boutons sur les petits écrans */
    }

    .button-retrait {
        background-color: #6c757d; /* Gris foncé pour le bouton Retrait */
    }

    .button-depot {
        background-color: #ffc107; /* Jaune pour le bouton Dépôt */
    }

    .container-custom {
        background-color: #12343b; /* Gris clair pour le fond */
        padding: 1rem; /* py-4 ajusté pour tous les côtés */
        display: flex;
        flex-direction: column; /* empile les boutons verticalement sur les petits écrans */
        align-items: center; /* centre les boutons */
        padding-top: 3rem;
        padding-bottom: 3rem;
    }

    @media (min-width: 768px) {
        /* Sur les écrans plus larges, affiche les boutons côte à côte */
        .container-custom {
            flex-direction: row; /* retourne à une disposition horizontale */
            justify-content: space-around; /* espace entre les boutons */
        }
        .button {
            display: inline-block; /* boutons en ligne */
            width: auto; /* annule la pleine largeur */
            margin-bottom: 0; /* retire l'espace supplémentaire en bas */
        }
    }
</style>

<div class="container-custom">
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
