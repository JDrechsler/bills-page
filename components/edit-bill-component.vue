<template>

	<q-modal-layout>
		<q-toolbar slot="header">
			<q-btn flat round dense v-close-overlay icon="keyboard_arrow_left"></q-btn>
			<q-toolbar-title>
				Edit {{updatedBill.title}}
			</q-toolbar-title>
		</q-toolbar>

		<div class="layout-padding">
			<card-comp :propbill="updatedBill"></card-comp>

			<q-field label="Title:">
				<q-input v-model="updatedBill.title" type="text"></q-input>
			</q-field>

			<q-field label="Amount:">
				<q-input v-model="updatedBill.amount" type="number"></q-input>
			</q-field>

			<q-field label="Day of Month:">
				<q-input v-model="updatedBill.dayOfMonth" type="number"></q-input>
			</q-field>

			<q-field label="Paid Status:">
				<q-toggle v-model="updatedBill.isPaid" color='positive'></q-toggle>
			</q-field>

			<q-field label="Image Url:">
				<q-input v-model="updatedBill.imageUrl" type="url"></q-input>
			</q-field>

			<br>
			<q-btn color='positive' v-close-overlay class="full-width" @click="updateBill(updatedBill)">Save Changes to {{updatedBill.title}}</q-btn>
			<br>
			<br>
			<q-collapsible icon="delete" :label="'Delete ' + updatedBill.title + ' Bill'">
				<div>
					<q-btn color='negative' icon='delete' v-close-overlay @click="deleteBill(updatedBill)"></q-btn>
				</div>
			</q-collapsible>

		</div>

	</q-modal-layout>
</template>


<script>

// https://jsfiddle.net/mvwcqt43/

export default {
	props: {
		/**@type {Bill} */
		propbill: {
			type: Object,
			required: true
		}
	},
	data() {
		return {
			/**@type {Bill} */
			updatedBill: {}
		}
	},
	created() {
		this.updatedBill = Object.assign({}, this.propbill);
	},
	methods: {
		updateBill(/**@type {Bill} */bill) {
			if (this.updatedBill.title.trim()) {
				this.$billsRef.doc(bill.id).update(bill)
				console.log('updated', bill.title)
			}
		},
		deleteBill(/**@type {Bill} */bill) {
			this.$billsRef.doc(bill.id).delete()
			console.log('deleted', bill.title)
		}
	},
	computed: {
		/**@returns {Array<Bill>} */
		bills() { return this.$store.state.bills }
	}
}
</script>

<style scoped>
.q-modal-layout-content.col.scroll {
  background: linear-gradient(45deg, #6cfd9f, #6887ff);
}

.q-field-label * {
  color: rgb(0, 0, 0);
}

.q-if-addon,
.q-if-control,
.q-if-label {
  color: black;
}
</style>
