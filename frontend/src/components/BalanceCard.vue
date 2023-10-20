<style scoped>

h1 {
	@apply text-xl font-semibold block my-1;
}
</style>

<template>
  <main class="w-full mx-auto bg-gradient-to-r from-amber-500 to-amber-400 ring-0 ring-amber-600 px-1 py-5 rounded-xl flex gap-4 items-start justify-evenly">
	<p><i class="fa fa-arrow-left" style="font-size:60px;"></i><br>
		Outgoing</p>
	
		<section>
			<!-- <p class="text-lg">Balance</p> -->
			<h1>IBAN XXXXXXXXXXXXX</h1>
			<h1 style="font-size:30px;">{{ balances.current.toLocaleString() }} €</h1>
			<!-- <span class="w-full flex justify-between items-center">
				<small>{{ balances.trend.value.toLocaleString() }} EUR</small>
				<span class="flex items-end">
					<i class="block fas fa-caret-up text-amber-400 text-xl mr-1"></i>
					<small class="mb-1 block">{{ balances.trend.percent }}{{ balances.trend.prefix }}</small>
				</span>
			</span> -->
		</section>
		<p><i class="fa fa-arrow-right" style="font-size:60px;"></i><br>
		Incoming</p>
		<!-- <section class="grid place-items-center rounded-lg bg-gradient-to-r from-amber-500 to-amber-500 shadow shadow-gray-800 shadow-inner text-gray-300 px-2 py-3">
			<section class="flex flex-col text-center">
				<span class="text-sm">Your points</span>
				<span>{{ balances.points }}</span>
			</section>
		</section> -->
	</main>
</template>

<script setup>

import { computed } from 'vue'
import { useBalance } from '@/stores/balance'

const balance = useBalance()
const balances = computed(() => balance)

</script>
