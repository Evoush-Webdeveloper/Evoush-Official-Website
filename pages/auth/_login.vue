<template>
	<div>
		<div class="container">
			<div class="row justify-content-center mt-5">
				<div class="col-12 col-lg-12 col-xs-12 col-sm-12">
					<div class="float-right">
						<ColorModePicker/>
					</div>
				</div>
			</div>


			<div class="row justify-content-center">
				<div class="col-lg-12 col-xs-12 col-sm-12">
						<!-- <pre>
						{{username}}
						</pre> -->

						<!-- <h1>Pakai yang ini bor</h1> -->
					<div class="box">
						<form @submit.prevent="login">

						<Logo class="logo"/>

						<h5 class="text-white mt-3">Login Web Replika Evoush</h5>
						<p class="text-muted mt-3 mb-2"> Please enter your username and password!</p>
						<div v-if="loginFailed" class="alert alert-danger mb-2">
							Username atau Password Anda salah.
						</div>

							<input type="text" name="username" v-model="user.username" placeholder="Username">
							 <small v-if="validation.username" class="mt-2 text-danger">
                                {{ validation.username[0] }}
                            </small>
							<input type="password" name="password" v-model="user.password" placeholder="Password">
							<small v-if="validation.password" class="mt-2 text-danger">
                                {{ validation.password[0] }}
                            </small>
							<button type="submit" name="">
								<div v-if="loading">
									<div class="d-flex align-items-start">
										<strong>Loading...</strong>
										<div class="spinner-border ml-auto" role="status" aria-hidden="true"></div>
									</div>
								</div>

								<span v-else>
									Login
								</span>
							</button>
							<br>
							<a @click="historyBack()" class="back">Kembali</a>

							<nuxt-link to="/#members" class="home">Home Page</nuxt-link>
							<div class="col-md-12">
								<ul class="social-network social-circle">
									<li><a href="https://twitter.com/EvoushOfficial" class="twitter"><i class="bx bxl-twitter"></i></a></li>
									<li><a href="https://www.facebook.com/evoush.evoush.12" class="facebook"><i class="bx bxl-facebook"></i></a></li>
									<li><a href="https://www.instagram.com/evoush.official" class="instagram"><i class="bx bxl-instagram"></i></a></li>
									<li><a href="https://www.youtube.com/channel/UCIzNgeNDD58z8XNppkopwzw" class="linkedin"><i class="bx bxl-youtube"></i></a></li>
								</ul>
							</div>

							<div class="mt-5 col-md-12 col-xs-12 col-sm-12">
								<blockquote>
									Anda bisa edit profile web replika anda, terlebih dahulu melalui proses login di form login diatas. Untuk informasi mengenai login user web replika bisa, Klik/Tap tombol request login web replika di bawah. <br>
									<br>
									<small class="text-danger">*Ketik username resmi member evoush anda*</small>
									<input type="text" name="request-username" v-model="request.username" placeholder="username member evoush anda">
									<a @click="RequestLogin" class="btn btn-primary rounded-pill btn-request" target="_blank">Request Login Web Replika</a>
								</blockquote>
							</div>

						</form>
					</div>

				</div>
			</div>

			<div class="row justify-content-center">
				<div class="col-lg-12 col-xs-12 col-sm-12">
				</div>
			</div>

		</div>
	</div>
</template>

<script>
	import ColorModePicker from '@/components/molecules/ColorModePicker'
	import Logo from '~/components/molecules/Logo'

	export default {
		layout: 'auth',
		components: {
			ColorModePicker,
			Logo
		},

		async asyncData({params}){
			const username = params.username
			return {
				username,
				request: {
					username: ''
				}
			}
		},

		data(){
			return{
				user: {
					username: '',
					password: ''
				},
				validation: {},
				loginFailed: null,
				token: localStorage.getItem('token'),
				loading: null
			}
		},
		head(){
			return {
				title: 'Evoush::Member | Login'
			}
		},

		mounted(){
			this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token}`
			this.$axios.get('/user')
			.then(response => {
				// console.log(response.username)
				if(this.token){
					this.$swal({
						position: 'top-end',
						icon: 'success',
						title: `Anda telah login menggunakan username : ${response.username}`,
						showConfirmButton: false,
						timer: 1500
					})
					return this.$router.push({
						name: 'profile-username',
						params: {username: response.username}
					})
				}
			})
			.catch(error => {
				console.log(error.response)
			})
		},

		methods:{

			login(){
				this.loading = true
				let username = this.user.username
				let password = this.user.password
				this.$axios.post('/login', {
					username,
					password
				})
				.then(res => {
					// console.log(res)
					if(res.data.success){
						this.loading = false
						localStorage.setItem('token', res.data.token)
						localStorage.setItem('username', res.data.data.username)
						// this.$swal({
						// 	position: 'top-center',
						// 	icon: 'success',
						// 	title: `Login success, anda telah login sebagai <strong>${username}</strong>`,
						// 	showConfirmButton: false,
						// 	timer: 1500
						// })
						return this.$router.push({
							name:'profile-username',
							params: {username: res.data.data.username}
						})
					}

					this.loginFailed = true
					this.loading = false
				})
				.catch(err => {
					// console.log(err)
					this.loading = false
					this.validation = err.response.data
				})
			},

			historyBack(){
				return this.$router.back()
			},

			RequestLogin(){
				const username = this.request.username
				if(username === ""){
					this.$toast("Kolom username member evoush anda wajib di isi")
					this.getAlert("Kolom username member evoush anda wajib di isi", 'https://c.tenor.com/ACsq1KK1S-UAAAAM/monday-work.gif', 'https://e7.pngegg.com/pngimages/906/961/png-clipart-white-bricks-and-wall-background-clean-walls.png')
				}else{
					this.$toast(`Ok ! ${username}, Request anda akan segera kami proses`)
					window.open(`https://wa.me/6288222668778?text=Request%20informasi%20login,%20Username:%20${username}`)
				}
			},

			getAlert(message, gif, bg) {
				this.$swal({
					title: message,
					width: 600,
					padding: "3em",
					background: `#fff url(${bg})`,
					backdrop: `
					rgba(0,0,123,0.4)
					url("${gif}")
					left top
					no-repeat
					`
				});
			},
		}
	}
</script>


<style>
.box {
	width: 100%;
	height: auto;
	margin-top: 1rem!important;
	/*left: 25%;*/
	background: #191919;
	text-align: center;
	transition: 0.25s;
}

.box h5{
	font-size: 16px;
}

.box p{
	font-size: 13px;
}

.box input[type="text"],
.box input[type="password"] {
	border: 0;
	background: none;
	display: block;
	margin: 20px auto;
	text-align: left;
	border: 2px solid #3498db;
	padding: 10px 10px;
	width: 250px;
	outline: none;
	color: white;
	border-radius: 24px;
	transition: 0.25s
}

.box h1 {
	color: white;
	text-transform: uppercase;
	font-weight: 500
}

.box input[type="text"]:focus,
.box input[type="password"]:focus {
	width: 250px;
	border-color: #2ecc71
}

.box button[type="submit"] {
	border: 0;
	background: none;
	display: block;
	margin: 20px auto;
	text-align: center;
	border: 2px solid #2ecc71;
	padding: 15px 40px;
	outline: none;
	color: white;
	border-radius: 24px;
	transition: 0.25s;
	cursor: pointer;
	width:200px;
}

.box input[type="submit"]:hover {
	background: #2ecc71
}

.box .back{
	border: 0;
	background: none;
	display: block;
	margin: 20px auto;
	text-align: center;
	border: 2px solid salmon;
	padding: 14px 40px;
	outline: none;
	color: white;
	border-radius: 24px;
	transition: 0.25s;
	cursor: pointer
}
.box .back:hover{
	background: #E67E22;
}

.box .home{
	border: 0;
	background: none;
	display: block;
	margin: 20px auto;
	text-align: center;
	border: 2px solid salmon;
	padding: 14px 40px;
	outline: none;
	color: white;
	border-radius: 24px;
	transition: 0.25s;
	cursor: pointer
}
.box .home:hover{
	background:  #e74c3c;
}


.forgot {
	text-decoration: underline
}

ul.social-network {
	list-style: none;
	display: inline;
	margin-left: 0 !important;
	padding: 0
}

ul.social-network li {
	display: inline;
	margin: 0 5px
}

.social-network a.icoFacebook:hover {
	background-color: #3B5998
}

.social-network a.icoTwitter:hover {
	background-color: #33ccff
}

.social-network a.icoGoogle:hover {
	background-color: #BD3518
}

.social-network a.icoFacebook:hover i,
.social-network a.icoTwitter:hover i,
.social-network a.icoGoogle:hover i {
	color: #fff
}

a.socialIcon:hover,
.socialHoverClass {
	color: #44BCDD
}

.social-circle li a {
	display: inline-block;
	position: relative;
	margin: 0 auto 0 auto;
	border-radius: 50%;
	text-align: left;
	width: 20px;
	height: 50px;
	font-size: 20px
}

.social-circle li i {
	margin: 0;
	line-height: 50px;
	text-align: center
}

.social-circle li a:hover i,
.triggeredHover {
	transform: rotate(360deg);
	transition: all 0.2s
}

.social-circle i {
	color: #fff;
	transition: all 0.8s;
	transition: all 0.8s
}

input[name="request-username"]{
	width: 100%!important;
	margin-left: -.1rem!important;
}
.btn-request{
	font-size:14px;
	width:100%;
	height:50px;
	padding:12px;
	margin-bottom: 5rem;
}

.logo {
	margin-left: -2rem;
}
@media (min-width: 992px) {
	.box {
		width: 800px;
		height: auto;
		margin-top: 2rem!important;
		margin-bottom: 15rem!important;
		/*left: 25%;*/
		background: #191919;
		text-align: center;
		transition: 0.25s;
	}

	.box h5{
		font-size: 25px;
	}

	.box p{
		font-size: 18px;
	}

	.box input[type="text"],
	.box input[type="password"] {
		border: 0;
		background: none;
		display: block;
		/*margin: 20px auto;*/
		text-align: left;
		border: 2px solid #3498db;
		padding: 10px 10px;
		width: 350px;
		outline: none;
		color: white;
		border-radius: 24px;
		transition: 0.25s
	}

	.box h1 {
		color: white;
		text-transform: uppercase;
		font-weight: 500
	}

	.box input[type="text"]:focus,
	.box input[type="password"]:focus {
		width: 250px;
		border-color: #2ecc71
	}

	.box input[type="submit"] {
		border: 0;
		background: none;
		display: block;
		margin: 20px auto;
		text-align: center;
		border: 2px solid #2ecc71;
		padding: 15px 35px;
		outline: none;
		color: white;
		border-radius: 24px;
		transition: 0.25s;
		cursor: pointer;
		width:350px;
	}

	.box input[type="submit"]:hover {
		background: #2ecc71
	}

	.social-circle li a {
		display: inline-block;
		position: relative;
		margin: 0 auto 0 auto;
		border-radius: 50%;
		text-align: left;
		width: 50px;
		height: 50px;
		font-size: 35px
	}

	input[name="request-username"]{
		margin-left: .1rem!important;
		width: 100%!important;
	}

}
</style>