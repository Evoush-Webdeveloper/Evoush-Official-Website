<template>
	<div>
		<div v-if="!members">
			<h1>Member yang anda cari tidak terdaftar</h1>
		</div>
		<div v-for="member in members" class="row justify-content-center px-2 py-1 mx-auto mb-5">

			<!-- <div class="col-lg-12 col-xs-12 col-sm-12">
				<SocialSharing :socials="socials"/>
			</div> -->

			<div class="col-lg-10 col-xs-12 col-sm-12 mx-auto">
				<!-- Profile widget -->
				<div class="shadow rounded overflow-hidden profile">
					<!-- <pre>
						http://localhost:8000/storage/{{member.cover}}
					</pre> -->
					<div class="px-4 pt-5 pb-4 cover" :style="(member.cover) ? `background-image: url('https://raw.githubusercontent.com/evoush-products/bahan_evoush/main/migration_db/${member.cover}')` : 'background-image: url(https://coolwallpapers.me/picsup/5605343-internet-wallpapers.jpg)'">
						<div class="media">
							<div class="row justify-content-center">
								<div class="col-lg-12 col-xs-12 col-sm-12">
									<div v-if="member.avatar" class="container">
										<img :src="`https://app.evoush.com/storage/${member.avatar}`" alt="..."  class="rounded-circle mb-3 profile profile-overlay">
										<div class="middle">
											<a :href="`https://app.evoush.com/storage/${member.avatar}`" class="btn btn-sm btn-primary mb-2" target="_blank">
												<i class='bx bx-zoom-in'></i>
											</a>
										</div>
									</div>
									<div v-else>
										<img src="https://raw.githubusercontent.com/codesyariah122/bahan-evoush/main/images/profile/default.jpg" :alt="member.name" class="mb-3 profile profile-overlay rounded-circle" width="80" style="width:250px!important;">
									</div>
								</div>
								<div class="col-lg-8 col-xs-6 col-sm-6">
									<div class="media-body">
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
											<!-- <div v-if="member.achievements.includes('STAR SAPHIRE')">
												<span class="badge badge-primary">
													STAR SAPHIRE
												</span>
											</div>
											<div v-else-if="member.achievements.includes('SAPHIRE')">
												<span class="badge badge-success">
													SAPHIRE
												</span>
											</div>
											<div v-else>
												<span class="badge badge-success">
													No Achievements
												</span>
											</div> -->

											<!-- <span :class="`${member.achievements.includes('STAR SAPHIRE') ? 'badge badge-primary' : 'badge badge-success'}`"><i class='bx bx-medal bx-lg'></i> {{(member.achievements.includes("STAR SAPHIRE")) ? "STAR SAPHIRE" : "SAPHIRE"}}</span></p> -->

											<p class="small text-white"> <i class='bx bx-map'></i>
												{{member.city}} | {{member.province}}
											</p>
										</div>
									</div>
								</div>
							</div>
						</div>

					<!-- <pre>
						{{token}}
					</pre> -->

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
									<div v-if="followers.length > 0">
										<h5 class="font-weight-bold mb-0 d-block">{{followers.length}}</h5>
									</div>
									<div v-else>
										<h5 class="font-weight-bold mb-0 d-block">0</h5>
									</div>
								</div>
								<small class="text-muted"><i class='bx bx-group'></i> Members</small>
							</li>
							<li v-if="token" class="mb-2 mt-2">
								<nuxt-link :to="{name:'profile-username', params: {username: user.username}}" class="btn btn-success">My Profile</nuxt-link>
							</li>
							<li v-else class="mb-2 mt-2">
								<ol class="button" style="list-style: none;">
									<li>
										<nuxt-link to="/auth/login"  class="btn btn-primary">Login</nuxt-link>
									</li>
									<li>
										<nuxt-link :to="{name: 'member-join-sponsor', params:{sponsor: member.username}}" class="btn btn-outline-success mb-5 mt-5"><i class='bx bxs-user-check'></i>  Join Now</nuxt-link>
									</li>
								</ol>
							</li>
						</ul>
					</div>

					<div class="col-lg-12 col-xs-12 col-sm-12 mb-5 mt-3">
						<center>
							<span class="blockquote-footer text-primary">
								! Klik atau tap pada bagian icon tabs di bawah untuk melihat informasi selanjutnya <br>atau informasi detail lainnya.
							</span>
						</center>
					</div>
					<ProfileTabs :member="member" :samples="samples" :user="user"/>

				</div>
			</div>
		</div>
	</div>
</template>

<script>
	import ProfileTabs from './ProfileTabs'

	export default {
		props: ['members'],
		components: {
			ProfileTabs
		},

		head(){
			return {
				title: `Evoush::Member | ${this.members[0].username}`,
				link: [
				{rel: 'canonical', href: `https://evoush.com/member/${this.members[0].username}`}
				],
				meta: [
				{ hid: 'description', name: 'description', content: 'Evoush::Member'},
				{ hid: 'keywords', name: 'keywords', content: 'Evoush::Member | Web::Replika'},
				{ hid: 'author', name: 'author' , content: `${this.members[0].username} | Evoush::Member`},
				{ hid: 'og:type', property: 'og:type', content: 'website'},
				{ hid: 'og:url', property: 'og:url', content: `https://evoush.com/member/${this.members[0].username}`},
				{ hid: 'og:title', property: 'og:title', content: 'Evoush Indonesia | Evoush::Member'},
				{ hid: 'og:site_name', property: 'og:site_name', content: `${this.members[0].name} | ${this.members[0].username}`},
				{ hid: 'og:description', property: 'og:description', content: `${this.members[0].quotes}`},
				{ hid: 'og:image', property: 'og:image', content: `https://app.evoush.com/storage/${this.members[0].avatar}`},
				{ hid: 'og:image:width', property: 'og:image:width', content: '600'},
				{ hid: 'og:image:height', property: 'og:image:height', content: '598'}
				]
			}
		},

		data(){
			return {
				samples: [
					{id:1, url: 'https://raw.githubusercontent.com/evoush-products/evoush-express/master/public/images/gallery/branding/klev_branding1.jpeg'},
					{id:2, url: 'https://raw.githubusercontent.com/evoush-products/evoush-express/master/public/images/gallery/branding/klev_branding2.jpeg'}
				],
				credential: {
					username: localStorage.getItem('username') ? localStorage.getItem('username') : '',
					token: localStorage.getItem('token') ? localStorage.getItem('token') : ''
				},

				followers: null,
				loading: true,
				user: '',
				sapaan: ''
			}
		},

		mounted(){
			// console.log(this.credential)
			if(this.credential.token && this.credential.username){
				return this.$router.push({
					name: 'profile-username',
					params: {username: this.username}
				})
			}else{
				console.log("ANJING")
			}

			this.$axios.defaults.headers.common.Authorization = `Bearer ${this.credential.token}`
			this.$axios.get('https://app.evoush.com/api/user')
			.then(response => {

                    // console.log(response.data.name)
                    this.user = response

                })
			.catch(error => {
				console.log(error.response.data)
			})

			this.getFollowers(this.members[0].username)

			// $crisp.push(['do', 'chat:hide'])
		},
		methods: {

			getCredential(){
				this.$store.commit('credential')
			},

			getFollowers(username){
				this.$axios.get(`https://app.evoush.com/api/member/join/active/${username}`)
				.then( res => {
					this.followers = res.data
					// console.log(this.followers.length)
					// console.log(this.length)
				})
				.catch(err => console.log(err.response))
				.finally(() => this.loading = false)
			},

			logout(){
				this.$swal({
					title: 'Are you sure?',
					text: "Want to exit this account",
					icon: 'warning',
					showCancelButton: true,
					confirmButtonColor: '#3085d6',
					cancelButtonColor: '#d33',
					confirmButtonText: 'Yes, Logout!'
				}).then((result) => {
					if (result.isConfirmed) {
						this.$swal(
							'Okay',
							'Anda akan segera logout',
							'success'
							)
						this.$axios.defaults.headers.common.Authorization  = `Bearer ${this.credentialUser.token}`
						this.$axios.post('/logout')
						.then(res => {
							// console.log(res)
							if(res.data.success) {

		                    //remove localStorage
		                    // localStorage.removeItem('token')
		                    this.credentialUser.token = localStorage.removeItem('token')
		                    this.credentialUser.username = localStorage.removeItem('username')
		                    //redirect ke halaman login
		                    return this.$router.push({
		                    	path: '/auth/login'
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
	margin-top: 2rem;
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
	left: 50%;
	transform: translate(-50%, -50%);
	-ms-transform: translate(-50%, -50%);
	text-align: center;
}

.middle i{
	font-size: 35px!important;
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
	margin-left:1rem;
	margin-top: 2rem;
}

@media (min-width: 992px) {
	.media .profile{
		margin-top: 10rem;
		margin-left: 10rem;
		width: 250px;
		height: auto;
	}

	.media-body{
		margin-top: 5rem;
		margin-left: -3rem;
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
		left: 85%;
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
		margin-left: 10rem;
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
		margin-left: 5rem;
		width: 350px;
		height: auto;
	}

	.media-body{
		margin-top: 5rem;
		/*margin-left: 3rem;*/
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
		left: 75%;
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
</style>