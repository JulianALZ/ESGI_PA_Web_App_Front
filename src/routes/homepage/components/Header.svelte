<script>
	let searchTerm = '';

	function handleProfileClick() {
		// Ajoutez ici votre logique pour le bouton de profil
		console.log('Profile button clicked');
	}

	function handleMenuClick() {
		// Ajoutez ici votre logique pour le bouton de menu
		console.log('Menu button clicked');
	}



	function handleSearch() {
		// Ajoutez ici votre logique pour la recherche
		console.log('Search initiated for:', searchTerm);
		// Effectuez l'action de recherche appropriée
	}
</script>

<nav class="navbar">
	<div class="container-fluid">
		<button class="btn icon-button me-3" on:click={handleProfileClick}>
			<img src="../../../../src/assets/user.png" alt="Menu" />
		</button>

		<div class="search-bar mx-auto">
			<input
				class="form-control search-input"
				type="search"
				placeholder="Recherche"
				aria-label="Search"
				bind:value={searchTerm}
				on:keyup={(e) => e.key === 'Enter' && handleSearch()}
			/>
		</div>

		<button class="btn icon-button ms-3" on:click={handleMenuClick}>
			<img src="../../../../src/assets/charte-graphique.png" alt="Profile" />
		</button>
	</div>
</nav>

<style>
    .navbar {
        background-color: transparent; /* Fond transparent */
        justify-content: space-between; /* Élément de flex pour aligner les enfants */
    }

    .btn {
        background-color: #f8f9fa; /* Gris clair pour les boutons */
        border-radius: 90%; /* Bordures arrondies */
    }

    .icon-button img {
        width: 24px;
        height: 24px;
    }

    .search-bar {
        flex-grow: 1; /* Permet à la barre de recherche de grandir */
    }

    .search-input {
        background-color: #f8f9fa; /* Gris clair pour la barre de recherche */
        border-radius: 20px; /* Bordures arrondies */
        border: 1px solid #ced4da; /* Bordure subtile */
    }
</style>
