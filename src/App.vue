<template>
	<section id="app">
		<h1>Стороны и объект договора</h1>
		<form class="form">
			<p class="form_header">Участник договора</p>
			<contractor-data
					v-for="(contractor,index) in contractors"
					:key="index"
					:id="index"
					:name="contractor.name"
					:phone="contractor.phone"
					@return_contractor_data="addReturnContractor"
			></contractor-data>
			<div class="button" @click="addContractor">+</div>
			<p class="form_header">Вид собственности</p>
			<select v-model="activeOption" class="selector">
				<option v-for="option in options" :value="option.value">
					{{ option.text }}
				</option>
			</select>
			<object-data v-if="activeOption" :change="activeOption"
			             @return_object_data="addReturnObjectData"></object-data>
			<table-rooms :change="activeOption" @return_table_rows="addReturnTableRows"></table-rooms>
			<div class="button_wrapper">
				<button class="button" type="button" @click="send">Send</button>
				<button class="button" type="button" @click="dataGenerator ">Load</button>
			</div>
		</form>
	</section>
</template>

<script>
	import ContractorData from '@/components/ContractorData'
	import ObjectData from "@/components/ObjectData";
	import TableRooms from "@/components/TableRooms";

	var contractorNextId = 0
	export default {
		name: 'app',
		components: {
			TableRooms,
			ObjectData,
			ContractorData
		},
		mounted() {
			this.indexator()
		},
		data() {
			return {
				objectData: [],
				rooms: [],
				activeOption: '',
				options: [
					{text: 'Частная', value: 'individual'},
					{text: 'Общественная', value: 'communal'},
					{text: 'Корпоративная', value: 'corporate'}],
				contractors: []
			};
		},
		methods: {
			dataGenerator() {
				var index = 0,
					contractor = []
				this.activeOption = activeOptionGenerator(this.options)
				namesGenerator().forEach((item) => {
					contractor.push({name: item, id: index++, phone: phoneGenerator()})
				})
				this.contractors = contractor
			},

			//добавляет пустые инпуты контрактора
			addContractor() {
				this.contractors.push({id: contractorNextId++, name: null, phone: null})
			},

			//Изменяет данные contractors в App  введёнными в дочернем ContractorData
			addReturnContractor(contractor) {
				this.contractors.forEach((item) => {
					if (item.id === contractor.id) {
						this.contractors[item.id].phone = contractor.phone
						this.contractors[item.id].name = contractor.name
					}
				})
			},
			addReturnTableRows(rows) {
				this.rooms = rows
			},
			addReturnObjectData(data) {
				this.objectData = data
			},
			//индексация contractors
			indexator() {
				this.contractors.forEach(() => {
					this.contractors[contractorNextId].id = contractorNextId
					contractorNextId++
				})
			},
			send() {
				this.$http.post('http://localhost:3000/formdata',
					{
						'activeOption': this.activeOption,
						'contractors': this.contractors,
						'rooms': this.rooms,
						'objectData': this.objectData
					}
				).then(response => {
					return response.json()
				}).then(newObj => {
					alert(JSON.stringify(newObj))
				})
			}
		}
	}

	function namesGenerator() {
		var num = Math.floor(Math.random() * 9) + 1,
			nameArr = ["Женя", "Вася", "Петя", "Маша", "Даша", "Саша", "Юля", "Анна", "Игорь"],
			result = []
		for (var i = 0; i < num; i++) {
			result.push(nameArr[Math.floor(Math.random() * 9)])
		}
		return result
	}

	function phoneGenerator() {
		var phone = '89'
		for (var i = 0; i < 9; i++) {
			phone += Math.floor(Math.random() * 9)
		}
		return phone
	}

	function activeOptionGenerator(options) {
		return options[Math.floor(Math.random() * 3)].value
	}
</script>

<style lang="scss">
	@import "assets/styles/form";
	@import "assets/styles/main";

</style>
