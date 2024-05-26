<script>
	async function Register(){
		const username = document.getElementById('EmailID').value;
		const password = document.getElementById('PasswordID1').value;
		const password2 = document.getElementById('PasswordID2').value;

		try {
			if(password === password2){
				const response = await fetch('https://esgi-pa-web-app-back.vercel.app/api/register', {
					method: 'POST',
					headers: {
						'Content-Type': 'application/json'
					},
					body: JSON.stringify({ username, password })
				});

				if (!response.ok) {
					throw new Error(await response.text());
				}

				window.location.href = '/login';
			}else{
				alert("Les deux mot de passe sont différents")
			}
		} catch (error) {
			console.log(error.message);
		}
	}
</script>

<style>
	.gradient-custom {
		/* fallback for old browsers */
		background: #6a11cb;

		/* Chrome 10-25, Safari 5.1-6 */
		background: -webkit-linear-gradient(to right, rgba(106, 17, 203, 1), rgba(37, 117, 252, 1));

		/* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
		background: linear-gradient(to right, rgba(106, 17, 203, 1), rgba(37, 117, 252, 1))
	}


	.card-registration .select-input.form-control[readonly]:not([disabled]) {
		font-size: 1rem;
		line-height: 2.15;
		padding-left: .75em;
		padding-right: .75em;
	}
	.card-registration .select-arrow {
		top: 13px;
	}

</style>


<section class="vh-100 gradient-custom">
	<div class="container h-100">
		<div class="row d-flex justify-content-center align-items-center h-100">
			<div class="col-lg-12 col-xl-11">
				<div class="card text-black" style="border-radius: 25px;">
					<div class="card-body p-md-5">
						<div class="row justify-content-center">
							<div class="col-md-10 col-lg-6 col-xl-5 order-2 order-lg-1">

								<p class="text-center h1 fw-bold mb-5 mx-1 mx-md-4 mt-4">Sign up</p>

								<form class="mx-1 mx-md-4">

									<div class="d-flex flex-row align-items-center mb-4">
										<i class="fas fa-user fa-lg me-3 fa-fw"></i>
										<div data-mdb-input-init class="form-outline flex-fill mb-0">
											<input type="text" id="NameID" class="form-control" />
											<label class="form-label" for="form3Example1c">Your Name</label>
										</div>
									</div>

									<div class="d-flex flex-row align-items-center mb-4">
										<i class="fas fa-envelope fa-lg me-3 fa-fw"></i>
										<div data-mdb-input-init class="form-outline flex-fill mb-0">
											<input type="email" id="EmailID" class="form-control" />
											<label class="form-label" for="form3Example3c">Your Email</label>
										</div>
									</div>

									<div class="d-flex flex-row align-items-center mb-4">
										<i class="fas fa-lock fa-lg me-3 fa-fw"></i>
										<div data-mdb-input-init class="form-outline flex-fill mb-0">
											<input type="password" id="PasswordID1" class="form-control" />
											<label class="form-label" for="form3Example4c">Password</label>
										</div>
									</div>

									<div class="d-flex flex-row align-items-center mb-4">
										<i class="fas fa-key fa-lg me-3 fa-fw"></i>
										<div data-mdb-input-init class="form-outline flex-fill mb-0">
											<input type="password" id="PasswordID2" class="form-control" />
											<label class="form-label" for="form3Example4cd">Repeat your password</label>
										</div>
									</div>

									<div class="form-check d-flex justify-content-center mb-5">
										<input class="form-check-input me-2" type="checkbox" value="" id="form2Example3c" />
										<label class="form-check-label" for="form2Example3">
											I agree all statements in <a href="#!">Terms of service</a>
										</label>
									</div>

									<div class="d-flex justify-content-center mx-4 mb-3 mb-lg-4">
										<button type="button" data-mdb-button-init data-mdb-ripple-init class="btn btn-primary btn-lg" on:click={Register}>Register</button>
									</div>

									<div>
										<p class="mb-0">Already have an account? <a href="/login" class="fw-bold">Sign in</a>
										</p>
									</div>

								</form>

							</div>
							<div class="col-md-10 col-lg-6 col-xl-7 d-flex align-items-center order-1 order-lg-2">

								<img src="../../../src/assets/logo.png" class="img-fluid" alt="Sample image">

							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</section>