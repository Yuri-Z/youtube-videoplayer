<template>
	<div class="main-page">
		<div class="search-interface">
			<input type="text" v-model="requestText" @keyup.enter="runSearch" placeholder="Поиск">
			<input type="button" @click="runSearch" value="Поиск">
			<br>
			<input type="button" @click="clear" value="Очистить">
		</div>
		<div class="response-list">
			<div class="response-list__item" v-for="(item, id) in response">
				<nuxt-link :to="'/' + id">{{item.title}}</nuxt-link>
				<div class="response-list__item-desc">
					{{item.description}}
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		components: {},
		data() {
			return {
				requestText: '',
				request: {
					url: "https://www.googleapis.com/youtube/v3/search",
					params: {
						part: "snippet",
						key: "AIzaSyACDbnvwjKF0A-xWxDHKShZeUjgDxAM_rg",
						maxResult: 10,
						type: "video",
						q: ''
					}
				},
				response: []
			}
		},
		methods: {
			runSearch() {
				this.request.params.q = this.requestText;

				this.$axios.get(this.request.url, {params: this.request.params})
					.then(response => {
						let resp = {};
						for (let index in response.data.items) {
							let item = response.data.items[index];
							resp[item.id.videoId] = {
								title: item.snippet.title,
								description: item.snippet.description,
							};
							this.response = resp;
						}
					})
					.catch(error => {
						console.log("error", error);
					});
			},
			clear() {
				this.response = [];
				this.requestText = '';
			}
		},
		mounted() {

		}
	}
</script>

<style src="../assets/styles/main-page.scss" lang="scss"/>
