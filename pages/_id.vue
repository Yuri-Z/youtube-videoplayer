<template>
	<div class="main-page">
		<iframe :src="'https://www.youtube.com/embed/' + this.video.id" width="800" height="480"></iframe>
		<div class="title">{{this.video.title}}</div>
		<div class="description">{{this.video.description}}</div>
	</div>
</template>

<script>
	export default {
		name: "id",
		props: {
			id: String
		},
		data() {
			return {
				params: {
					part: 'snippet',
					id: this.$route.params.id,
					key: 'AIzaSyACDbnvwjKF0A-xWxDHKShZeUjgDxAM_rg',
					maxResult: 10,
					type: "video",
				},
				video: {
					id: undefined,
					title: undefined,
					description: undefined
				}
			}
		},
		mounted() {
			this.$axios.get('https://www.googleapis.com/youtube/v3/videos', {params: this.params})
				.then(response => {
					let item = response.data.items[0];
					console.log(item);
					this.video = {
						id: item.id,
						title: item.snippet.title,
						description: item.snippet.description,
					}
				})
				.catch(error => {
					console.log("error", error);
				});
		},
	}
</script>

<style lang="scss">
	.title {
		font-size: 30px;
	}
	.description {
		padding-top: 20px;
	}
</style>