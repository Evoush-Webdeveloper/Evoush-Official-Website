<template>
	<div>
		<div v-for="member in members" class="row justify-content-center px-2 py-1 mx-auto mb-5">
			<div class="col-lg-10 col-xs-12 col-sm-12 mx-auto">
				<button class="btn btn-sm btn-primary mt-3 mb-2" @click="openUpdateCover"><i class='bx bxs-edit bx-lg'></i> Update Cover Profile</button>

				<div v-if="editCover" class="mt-2">
					<i @click="closeCover" class='bx bx-window-close lg mt-2' style="font-size: 30px; cursor: pointer;"></i>
					<form class="mb-5" width="200px" name="myForm" method="post" @submit.prevent="updateCover" enctype="multipart/form-data">
						<input type="hidden" name="id" id="id_member" :value="member.id">
						<label for="file">Pilih design banner atau cover yang ingin anda upload</label>
						<input  class="form-control mb-2" type="file" ref="file" id="file" name="cover" accept="cover/*" @change="fileCover">
						<button class="btn btn-sm btn-primary">Upload</button>
					</form>
				</div>

				<div v-if="message_cover">
					<div class="alert alert-success alert-dismissible fade show" role="alert">
						<strong>{{message_cover}}</strong> <br> Cover Profile Baru anda <strong class="text-info">{{cover}}</strong>.
						<button type="button" class="close" data-dismiss="alert" aria-label="Close">
							<span aria-hidden="true">&times;</span>
						</button>
					</div>
				</div>

				<div class="shadow rounded overflow-hidden profile">

					<div  class="px-4 pt-5 pb-4 cover" :style="(member.cover) ? `background-image: url('https://app.evoush.com/storage/${member.cover}')` : 'background-image: url(https://coolwallpapers.me/picsup/5605343-internet-wallpapers.jpg)'">


						<div class="media align-items-end">
							<div class="row justify-content-center">
								<div class="col-lg-12 col-xs-12 col-sm-12">
									<div v-if="member.avatar" class="container">
										<img :src="`https://app.evoush.com/storage/${member.avatar}`" alt="..." width="130" class="rounded-circle mb-3 profile profile-overlay">

										<div class="middle">
											<button class="btn btn-sm btn-primary ml-2 mb-2" @click="openUpdateAvatar"><i class='bx bxs-edit bx-lg'></i></button>
											<a :href="`https://app.evoush.com/storage/${member.avatar}`" class="btn btn-sm btn-primary mb-2" target="_blank"><i class='bx bx-zoom-in'></i></a>
										</div>
										<!-- <div v-if="preview">
											<img :src="preview" alt="..." width="130" class="rounded-circle mb-3 profile profile-overlay">
										</div>
										<div v-else>
											<div v-if="avatar">
												<img :src="`https://app.evoush.com/storage/${avatar}`" alt="..." width="130" class="rounded-circle mb-3 profile profile-overlay">
											</div>

											<img :src="`https://app.evoush.com/storage/${avatar}`" alt="..." width="130" class="rounded-circle mb-3 profile profile-overlay">

											<button class="btn btn-sm btn-primary mt-3 mb-2" @click="openUpdateAvatar"><i class='bx bxs-edit bx-lg'></i> Update Foto Profile</button>

										</div> -->
										<!-- <div class="middle">
											<div class="text" @click="openUpdateAvatar">
												<i class='bx bx-edit'></i> Update foto
											</div>

											<div v-if="editForm">
												<form width="200px" name="myForm" method="post" @submit.prevent="updateAvatar" enctype="multipart/form-data">
													<input type="hidden" name="id" id="id_member" :value="member.id">
													<input  class="form-control mb-2" type="file" ref="file" id="file" name="avatar" accept="avatar/*" @change="fileAvatar">
													<button class="btn btn-sm btn-primary">Upload</button>
												</form>
											</div>
										</div> -->

									</div>
									<div v-else>
										<img src="https://raw.githubusercontent.com/codesyariah122/bahan-evoush/main/images/profile/default.jpg" :alt="member.name" class="image--profile-member rounded-circle center-block d-block mx-auto mt-0 mb-0" width="100">

										<button class="btn btn-sm btn-primary mt-3 mb-2 edit-avatar" @click="openUpdateAvatar"><i class='bx bxs-edit bx-lg'></i> Update Foto Profile</button>
									</div>
								</div>
								<div class="col-lg-12 col-xs-12 col-sm-12 mt-3">
									<div class="container-fluid media-body">
										<!-- form upload -->
										<div v-if="editForm" class="mt-2 mb-5" style="background-color: #fff;">
											<div class="container-fluid form-edit-profile">
												<i @click="closeForm" class='bx bx-window-close lg mt-2' style="font-size: 30px; cursor: pointer;"></i>
												<form width="200px" name="myForm" method="post" @submit.prevent="updateAvatar" enctype="multipart/form-data">
													<input type="hidden" name="id" id="id_member" :value="member.id">
													<label for="file">Pilih foto yang ingin di upload, kemudian klik tombol Upload</label>
													<input  class="form-control mb-2" type="file" ref="file" id="file" name="avatar" accept="avatar/*" @change="fileAvatar">
													<button class="btn btn-sm btn-primary mt-2">Upload</button>
													<button @click="closeForm" class="btn btn-sm btn-secondary mt-2">Cancel</button>
													<br><br>
												</form>
											</div>
										</div>
										<!-- end form upload -->

										<h4 style="text-transform: capitalize;">{{member.name}}</h4>
										<p class="mt-2 mb-3">
											<!-- <pre>
												{{member.achievements}}
											</pre> -->
											<div v-if="member.achievements">
												<span :class="`${member.achievements.includes('STAR SAPHIRE') ? 'badge badge-primary mb-3' : 'badge badge-success mb-3'}`"><i class='bx bx-medal bx-lg'></i> {{(member.achievements.includes("STAR SAPHIRE")) ? "STAR SAPHIRE" : "SAPHIRE"}}</span>
											</div>
											<div v-else>
												<span class="badge badge-danger mb-3">
													No Achievements
												</span>
											</div>
										</p>
										<p class="small text-white"> <i class='bx bx-map'></i>
											{{member.city}} | {{member.province}}
										</p>
									</div>
								</div>
							</div>
						</div>
					</div>

					<div class="p-4 d-flex justify-content-end text-center">
						<ul class="list-inline mb-0">
							<li class="list-inline-item">
								<h5 class="font-weight-bold mb-0 d-block">
									{{samples.length}}
								</h5><small class="text-muted"><i class='bx bx-images'></i> Gallery</small>
							</li>
							<li class="list-inline-item">
								<div v-if="loading">
									<img src="https://c.tenor.com/I6kN-6X7nhAAAAAj/loading-buffering.gif" width="50">
								</div>
								<div v-else>
									<div v-if="followers > 0">
										<h5 class="font-weight-bold mb-0 d-block">{{followers}}</h5>
									</div>
									<div v-else>
										<h5 class="font-weight-bold mb-0 d-block">0</h5>
									</div>
								</div>
								<small class="text-muted"><i class='bx bx-group'></i> Members</small>
							</li>
							<li class="mb-2 mt-2">
								<a class="btn btn-danger" @click.prevent="logout">Logout</a>
							</li>
							<li>
								<nuxt-link :to="{name:'profile-edit-username', params: {username: user.username}}" class="btn btn-outline-info btn-sm mb-0 mt-3">Edit profile</nuxt-link>
							</li>
						</ul>
					</div>

					<div class="container-fluid">
						<div class="row justify-content-center">
							<div class="col-lg-12 col-xs-12 col-sm-12">
								<center>
									<div v-if="newMembers.length > 0" class="mb-5 mt-2 alert alert-warning alert-dismissible fade show" role="alert">
										<strong>Halo {{member.username}}!</strong> anda mempunyai <strong>{{newMembers.length}} member</strong> yang belum di aktivasi, click tombol list member di bawah untuk melanjutkan aktivasi member anda. <br>

										<nuxt-link to="/profile/new-member/activated" class="btn btn-lg btn-primary">List Member</nuxt-link>

										<button type="button" class="close" data-dismiss="alert" aria-label="Close">
											<span aria-hidden="true">&times;</span>
										</button>
									</div>

									<div v-if="message">
										<div class="alert alert-success alert-dismissible fade show" role="alert">
											<strong>{{message}}</strong> <br> Profile Image Baru anda <strong class="text-info">{{avatar}}</strong>.
											<button type="button" class="close" data-dismiss="alert" aria-label="Close">
												<span aria-hidden="true">&times;</span>
											</button>
										</div>
									</div>

									<div class="alert alert-success alert-dismissible fade show" role="alert">
										<strong>Halo! {{sapaan}}</strong> Anda telah login menggunakan username <strong class="text-info">{{username}}</strong>.
										<button type="button" class="close" data-dismiss="alert" aria-label="Close">
											<span aria-hidden="true">&times;</span>
										</button>
									</div>
								</center>
							</div>
						</div>
					</div>


					<ProfileTabs :user="user" :samples="samples"  :sapaan="sapaan" :username="username"/>

				</div>
			</div>
		</div>
	</div>
</template>

<script>
	import ProfileTabs from './ProfileTabs'

	export default {
		props: ['members', 'token', 'user', 'username'],
		components: {
			ProfileTabs
		},

		data(){
			return {
				samples: [
				{id:1, url: 'https://raw.githubusercontent.com/evoush-products/evoush-express/master/public/images/gallery/branding/klev_branding1.jpeg'},
				{id:2, url: 'https://raw.githubusercontent.com/evoush-products/evoush-express/master/public/images/gallery/branding/klev_branding2.jpeg'}
				],
				newMembers: [],
				followers: null,
				loading: true,
				sapaan: '',
				user: '',
				avatar: null,
				cover: null,
				message: null,
				message_cover: null,
				preview: null,
				errors: null,
				editForm: false,
				editCover: false
			}
		},
		head(){
			return {
				title: `Evoush::Profile | ${this.members[0].username}`,
				meta: [
				{ hid: 'description', name: 'description', content: 'Evoush::Member'},
				{ hid: 'keywords', name: 'keywords', content: 'Evoush::Official | Web::Replika'},
				{ hid: 'author', name: 'author' , content: `${this.members[0].username} | Evoush::Member`},
				{ hid: 'og:type', property: 'og:type', content: 'website'},
				{ hid: 'og:url', property: 'og:url', content: `https://evoush.com/member/${this.members[0].username}`},
				{ hid: 'og:title', property: 'og:title', content: 'Evoush Indonesia | Evoush::Member'},
				{ hid: 'og:site_name', property: 'og:site_name', content: `${this.members[0].name} | ${this.user.username}`},
				{ hid: 'og:description', property: 'og:description', content: `${this.members[0].quotes}`},
				{ hid: 'og:image', property: 'og:image', content: `https://app.evoush.com/storage/${this.members[0].avatar}`},
				{ hid: 'og:image:width', property: 'og:image:width', content: '600'},
				{ hid: 'og:image:height', property: 'og:image:height', content: '598'}
				]
			}
		},

		mounted(){
			if(!this.token){
				this.$swal({
					icon: 'error',
					title: 'Oops...',
					text: 'Sepertinya anda belum login!',
					footer: '<a href="https://evoush.com/issue">Why do I have this issue?</a>'
				})
				return this.$router.push({
					path: '/auth/login'
				})
			}

			this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token}`
			this.$axios.get(`/member/${this.username}`)
			.then(response => {
				// console.log(response)
				this.user = response.data[0]
				// console.log(this.user)
				let h=(new Date()).getHours();
				let m=(new Date()).getMinutes();
				let s=(new Date()).getSeconds();
				if (h >= 4 && h < 10) this.sapaan = "Selamat pagi, "
					if (h >= 10 && h < 15) this.sapaan = "Selamat siang, "
						if (h >= 15 && h < 18) this.sapaan = "Selamat sore, "
							if (h >= 18 || h < 4) this.sapaan = "Selamat malam, "
						})
			.catch(error => {
				console.log(error.response.data)
			})

			this.getFollowers(this.members[0].username),
			this.inActiveMember()
		},
		methods: {
			getFollowers(username){
				this.$axios.$get(`/member/join/active/${username}`)
				.then( res => {
					if(res.length > 0){
						this.followers = res.length
					}else{
						this.followers = 0
					}
					// console.log(this.length)
				})
				.catch(err => console.log(err.response))
				.finally(() => this.loading = false)
			},

			inActiveMember(){
				this.$axios.get(`/member/join/inactive/${this.members[0].username}`)
				.then(res=>{
					this.newMembers = res.data
					if(this.newMembers.length > 0){
						this.$toast(`Halo ${this.members[0].username}, Anda mempunya ${this.newMembers.length} member yang perlu di aktivasi. Silahkan diaktivasi.`)
					}
				})
			},

			closeForm(){
				this.editForm = false
			},

			openUpdateAvatar(){
				this.editForm = true
			},

			fileAvatar(event){
				this.avatar = event.target.files[0]
				this.preview = URL.createObjectURL(event.target.files[0])
			},

			updateAvatar(){
				const id = document.querySelector('#id_member').value
				let formData = new FormData
				formData.append("avatar", this.avatar)

				this.$axios.post(`/member/update/avatar/${id}`, formData, {
					headers: {'Content-Type': 'application/x-www-form-urlencoded'}
				})
				.then(res=>{
					// console.log(res.data)
					this.$swal({
						position: 'top-end',
						icon: 'info',
						title: this.message,
						showConfirmButton: false,
						timer: 1500
					})
					this.editForm = false
					this.message = res.data.message
					this.avatar = res.data.data
				})
			},

			openUpdateCover(){
				this.editCover = true
			},

			fileCover(e){
				this.cover = e.target.files[0]
			},

			closeCover(){
				this.editCover = false
			},

			updateCover(){
				this.$swal({
					title: 'Do you want to save the changes?',
					showDenyButton: true,
					showCancelButton: true,
					confirmButtonText: `Save`,
					denyButtonText: `Don't save`,
				}).then((result) => {
					/* Read more about isConfirmed, isDenied below */
					if (result.isConfirmed) {
						const id = document.querySelector('#id_member').value
						let formData = new FormData
						formData.append("cover", this.cover)
						this.$axios.post(`/member/update/cover/${id}`, formData, {
							headers: {'Content-Type': 'application/x-www-form-urlencoded'}
						})
						.then(res=>{
							// console.log(res.data)
							this.$swal({
								position: 'top-end',
								icon: 'success',
								title: this.message,
								showConfirmButton: false,
								timer: 1500
							})
							this.editCover = false
							this.message_cover = res.data.message
							this.cover = res.data.data
							location.reload()
						})
					} else if (result.isDenied) {
						this.editCover = false
						this.$swal('Changes are not saved', '', 'info')
					}
				})

			},


			logout(){
				this.$swal({
					title: 'Anda yakin ingin keluar ?',
					text: `Anda akan keluar dari akun ${this.user.username}`,
					icon: 'warning',
					showCancelButton: true,
					confirmButtonColor: '#3085d6',
					cancelButtonColor: '#d33',
					confirmButtonText: 'Yes, Logout!'
				}).then((result) => {
					if (result.isConfirmed) {
						this.$swal(
							`Bye~ ${this.user.username}`,
							`Anda telah logout dari akun ${this.user.username}`,
							'success'
							)
						this.$axios.defaults.headers.common.Authorization  = `Bearer ${this.token}`
						this.$axios.post('/logout')
						.then(res => {
							// console.log(res)
							if(res.data.success) {

		                    //remove localStorage
		                    localStorage.removeItem('token')
		                    localStorage.removeItem('username')

		                    //redirect ke halaman login
		                    return this.$router.push({
		                    	name: 'auth-login',
		                    	params: {
		                    		username: this.user.username
		                    	}
		                    })
		                }
		            })
						.catch(error => {
							// console.log(error)
							console.log(error.response.data)
						})
					}
				})
			}
		}
	}
</script>



<style scoped>
.cover {
	background-repeat: no-repeat;
	/*height: 50vh;*/
	min-height: 50vh;
	width: 100%;
	-webkit-background-size: cover;
	background-size: cover;
	position: relative;
}

.media .profile{
	width: 200px;
	height: auto;
	border-radius: 50%!important;
	margin-top: 1rem;
	margin-left: -3rem;
}

.container {
	position: relative;
	width: 50%;
}
.profile-overlay{
	opacity: 1;
	display: block;
	width: 100%;
	height: auto;
	transition: .5s ease;
	backface-visibility: hidden;
}
.container:hover .profile-overlay{
	opacity: 0.3;
}

.middle {
	transition: .5s ease;
	opacity: 0;
	position: absolute;
	top: 50%;
	left: 40%;
	transform: translate(-50%, -50%);
	-ms-transform: translate(-50%, -50%);
	text-align: center;
}

.container:hover .middle{
	opacity: 1;
}

.text {
	background-color: #04AA6D;
	color: white;
	font-size: 12px;
	padding: 2px 3px;
	cursor: pointer;
}

.media-body {
	margin-left:-1rem;
	margin-top: 1rem;
}

@media (min-width: 992px) {
	.media .profile{
		margin-top: 10rem;
		margin-left: 11.5rem;
		width: 250px;
		height: auto;
	}

	.media-body{
		margin-top:2rem;
		margin-left: 3rem;
	}

	.media-body h4{
		font-size:31px;
	}

	.media-body p{
		font-size: 21px;
	}

	.profile-overlay{
		opacity: 1;
		display: block;
		width: 100%;
		height: auto;
		transition: .5s ease;
		backface-visibility: hidden;
	}
	.container:hover .profile-overlay{
		opacity: 0.3;
	}

	.middle {
		transition: .5s ease;
		opacity: 0;
		position: absolute;
		top: 50%;
		left: 68%;
		transform: translate(-50%, -50%);
		-ms-transform: translate(-50%, -50%);
		text-align: center;
	}

}

@media(min-width: 820px){
	.media .profile {
		width: 250px;
		margin-left: 2rem;
	}
	.media-body {
		margin-left: -1rem;
	}
	.media-body h4{
		font-size:33px;
	}
	.media-body p{
		font-size: 21px;
	}
	.media-body i{
		font-size: 16px;
	}
	.media-body span{
		font-size: 16px;
	}
	.middle {
		transition: .5s ease;
		opacity: 0;
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		-ms-transform: translate(-50%, -50%);
		text-align: center;
	}
	.middle i{
		font-size: 37px!important;
	}
}

@media (min-width: 884px) {
	.media .profile {
		width: 350px;
		margin-left: 1rem;
	}
	.media-body {
		margin-left: -1rem;
	}
	.media-body h4{
		font-size:33px;
	}
	.media-body p{
		font-size: 21px;
	}
	.media-body i{
		font-size: 16px;
	}
	.media-body span{
		font-size: 16px;
	}
	.middle {
		transition: .5s ease;
		opacity: 0;
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		-ms-transform: translate(-50%, -50%);
		text-align: center;
	}
	.middle i{
		font-size: 37px!important;
	}


	@media (min-width: 768px) {
		.media .profile {
			width: 350px;
			margin-left: .1rem;
		}
		.media-body {
			margin-left: -7rem;
		}
		.media-body h4{
			font-size:33px;
		}
		.media-body p{
			font-size: 21px;
		}
		.media-body i{
			font-size: 16px;
		}
		.media-body span{
			font-size: 16px;
		}
		.middle {
			transition: .5s ease;
			opacity: 0;
			position: absolute;
			top: 50%;
			left: 70%;
			transform: translate(-50%, -50%);
			-ms-transform: translate(-50%, -50%);
			text-align: center;
		}
		.middle i{
			font-size: 40px!important;
		}
	}

	@media(min-width: 1280px){
		.media .profile{
			margin-top: 10rem;
			margin-left: 3rem;
			width: 350px;
			height: auto;
		}

		.media-body{
			margin-top: 5rem;
			margin-left: 3rem;
		}

		.media-body h4{
			font-size:31px;
		}

		.media-body p{
			font-size: 21px;
		}

		.profile-overlay{
			opacity: 1;
			display: block;
			width: 100%;
			height: auto;
			transition: .5s ease;
			backface-visibility: hidden;
		}
		.container:hover .profile-overlay{
			opacity: 0.3;
		}

		.middle {
			transition: .5s ease;
			opacity: 0;
			position: absolute;
			top: 50%;
			left: 55%;
			transform: translate(-50%, -50%);
			-ms-transform: translate(-50%, -50%);
			text-align: center;
		}

		.middle i{
			font-size: 37px!important;
		}
	}

	@media(min-width: 2560px){
		.media .profile{
			margin-top: 10rem;
			margin-left: 25rem;
			width: 450px;
			height: auto;
		}

		.media-body{
			margin-top: 5rem;
			margin-left: 3rem;
		}

		.media-body h4{
			font-size:31px;
		}

		.media-body p{
			font-size: 21px;
		}

		.profile-overlay{
			opacity: 1;
			display: block;
			width: 100%;
			height: auto;
			transition: .5s ease;
			backface-visibility: hidden;
		}
		.container:hover .profile-overlay{
			opacity: 0.3;
		}

		.middle {
			transition: .5s ease;
			opacity: 0;
			position: absolute;
			top: 50%;
			left: 98%;
			transform: translate(-50%, -50%);
			-ms-transform: translate(-50%, -50%);
			text-align: center;
		}

		.middle i{
			font-size: 37px!important;
		}
	}
}
</style>