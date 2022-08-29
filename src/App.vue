<script>
	import axios from 'axios'

	export default {
		data() {
			return {
				list: [],
				searchTerm: '',
				activeList: 'filled'
			}
		},
		computed: {
			filtered() {
				return this.list.filter(item => {
					let term = new RegExp('(' + this.searchTerm + ')', 'i')
					return item.match(term)
				})
			}
		},
		watch: {
			activeList(val) {
				if (val == 'regular') {
					axios.get('/assets/fonts/FluentSystemIcons-Regular.json')
						.then(res => {
							this.list = res.data
							this.list = [this.list].map((item, index) => {
								return Object.keys(item)
							})[0]
						})
				} else {
					axios.get('/assets/fonts/FluentSystemIcons-Filled.json')
						.then(res => {
							this.list = res.data
							this.list = [this.list].map((item, index) => {
								return Object.keys(item)
							})[0]
						})
				}
			}
		},
		mounted() {
			axios.get('/assets/fonts/FluentSystemIcons-Filled.json')
				.then(res => {
					this.list = res.data
					this.list = [this.list].map((item, index) => {
						return Object.keys(item)
					})[0]
				})
		},
	}
</script>
<template>
	<main>
		<input type="text"
			class="form-control w-100 bg-secondary bg-opacity-10 form-control-sm rounded-fill border-0 fs-8"
			v-model="searchTerm" placeholder="Search for icon">
		<div class="d-flex">
			<div class="form-check">
				<input class="form-check-input" type="radio" name="filled" id="filled" value="filled"
					v-model="activeList">
				<label class="form-check-label" for="filled">
					Filled Icon
				</label>
			</div>
			<div class="form-check">
				<input class="form-check-input" type="radio" name="regular-icon" id="regular-icon" value="regular"
					v-model="activeList">
				<label class="form-check-label" for="regular-icon">
					Regular Icon
				</label>
			</div>
		</div>
		<ul class="d-flex flex-wrap">
			<li v-for="(icon, index) in filtered.slice(0,100)">
				<div class="preview">
					<span class="inner">
						<i v-if="activeList == 'filled'" :class="'vf-'+icon+' icon'"></i>
						<i v-if="activeList == 'regular'" :class="'vr-'+icon+' icon'"></i>
					</span>
					<br>
					<span class='label'>{{icon}}</span>
				</div>
			</li>
		</ul>
	</main>
</template>
<style scoped>
	.preview {
		width: 100px;
		display: inline-block;
		margin: 10px;
	}

	.preview .inner {
		display: inline-block;
		width: 100%;
		text-align: center;
		background: #f5f5f5;
		-webkit-border-radius: 3px 3px 0 0;
		-moz-border-radius: 3px 3px 0 0;
		border-radius: 3px 3px 0 0;
	}

	.preview .inner {
		line-height: 85px;
		font-size: 40px;
		color: #333;
	}

	.label {
		display: inline-block;
		width: 100%;
		box-sizing: border-box;
		padding: 5px;
		font-size: 10px;
		font-family: Monaco, monospace;
		color: #666;
		word-break: break-all;
		background: #ddd;
		-webkit-border-radius: 0 0 3px 3px;
		-moz-border-radius: 0 0 3px 3px;
		border-radius: 0 0 3px 3px;
		color: #666;
	}
</style>