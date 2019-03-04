<template>
	<div class="dropdown">
		<div class="dropdown__header" @click="toggleDropdown($event)">
			<span class="table_header">Площадь</span>...60м<span class="top_index">2</span>
			<span class="fa fa-angle-down" aria-hidden="true">⏷</span>
			<span class="fa fa-angle-up" aria-hidden="true">⏶</span>
		</div>
		<div class="dropdown__content">
			<table>
				<tr>
					<th>Жилая</th>
					<th>Нежилая</th>
					<th>Площадь с пониж. кооф</th>
				</tr>
				<tr v-for="row in rows">
					<td>{{row[0]}}</td>
					<td>{{row[1]}}</td>
					<td>{{row[2]}}</td>
				</tr>
			</table>
		</div>
	</div>
</template>

<script>
	export default {
		name: "TableRooms",
		props: ['change'],
		data() {
			return {
				randomData: ['Кухня', 'Коридор', 'Балкон', 'Прихожая', 'Санузел'],
				rows: []
			}
		},
		watch: {
			change() {
				this.rowPush()
			},
			rows() {
				this.returnData()
			}
		},
		methods: {
			
			returnData() {
				this.$emit('return_table_rows', {
					rows: this.rows
				})
			},
			rowPush() {
				this.rows = []
				var num = Math.round(Math.random() * 10 + 1)
				for (var i = 0; i < num; i++)
					this.rows.push(this.randomRow())
			},
			randomRow() {
				var arr = []
				for (var i = 0; i < 3; i++) {
					arr.push(this.randomData[
						Math.round(
							Math.random() * (this.randomData.length - 1)
						)])
				}
				return arr
			},
			toggleDropdown(event) {
				event.currentTarget.classList.toggle('is-active')
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import "../assets/styles/dropdown";
	@import "../assets/styles/table-rooms";

</style>
