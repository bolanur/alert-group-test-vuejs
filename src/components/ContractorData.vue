<template>
	<div class="contractor_wrapper">
		<div class="text_field_wrapper">
			<input type="text" class="text_field" v-model="contractor_name">
			<p class="text_field_error">{{nameValidator}}</p>
		</div>
		<div class="text_field_wrapper">
			<input type="text" class="text_field" v-model="contractor_phone">
			<p class="text_field_error">{{phoneValidator}}</p>
		</div>
	</div>
</template>

<script>
	export default {
		name: "ContractorData",
		props: ['id', 'name', 'phone'],
		data() {
			return {
				phoneValidator: 'Введите номер телефона',
				nameValidator: 'Введите имя',
				contractor_name: null,
				contractor_phone: null,
				contractor_id: null
			}
		},
		mounted() {
			this.contractor_name = this.name
			this.contractor_phone = this.phone
			this.contractor_id = this.id
		},
		watch: {
			contractor_name() {
				this.nameValidator = !(this.contractor_name) ? 'Введите имя' :
					/[^А-ЯA-Zа-яa-z\sЁё]/.test(this.contractor_name) ? 'Недопустимые символы в имени' : ''
				if (this.nameValidator === '')
					this.returnData()
			},
			contractor_phone() {
				this.phoneValidator = !(this.contractor_phone) ? 'Введите номер телефона' :
					/[^\d\s]/.test(this.contractor_phone) ? 'Недопустимые символы в номере телефона' :
						!/\b\d{11}\b/.test(this.contractor_phone.replace(/\s/g, '')) ? 'В номере телефона должно быть 11 цифр' : ''
				if (this.phoneValidator === '') {
					this.returnData()
				}
			}
		},
		methods: {
			returnData() {
				this.$emit('return_contractor_data', {
					id: this.contractor_id,
					name: this.contractor_name,
					phone: this.contractor_phone

				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import "../assets/styles/contractor";
</style>
