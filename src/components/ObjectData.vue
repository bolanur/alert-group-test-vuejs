<template>
	<div class="info">
		<p class="object"><span class="object_header">Альфа:</span> {{block}} блок</p>
		<p class="object"><span class="object_header">Квартира:</span> №{{appartament}}</p>
		<p class="object"><span class="object_header">Строительные оси:</span> {{constructionAxis}}</p>
	</div>
</template>

<script>
	export default {
		name: 'ObjectData',
		props: ['change'],
		mounted() {
			this.randomObjectData()
		},
		watch: {
			change() {
				this.randomObjectData()
			},
			constructionAxis(){
				this.returnData()
			}
		},
		data() {
			return {
				block: '',
				appartament: '',
				constructionAxis: ''
			}
		},
		methods: {
			returnData() {
				this.$emit('return_object_data', {
					block:this.block,
					appartament:this.appartament,
					constructionAxis:this.constructionAxis
				})
			},
			randomObjectData() {
				this.block = Math.floor(Math.random() * 5 + .5)

				this.appartament = Math.floor(Math.random() * 100 - .5)

				this.constructionAxis = charGenerator().toUpperCase() + charGenerator() + charGenerator() +
					'-' + Math.round(Math.random() * 100 - .5)
			}
		}
	}

	function charGenerator() {
		return String.fromCharCode(Math.floor(Math.random() * (97 - 119)) + 119)
	}
</script>

<style lang="scss" scoped>
	@import "../assets/styles/object-data.scss";
</style>
