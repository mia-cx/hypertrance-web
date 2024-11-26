<script lang="ts">
	import Money from '$src/lib/components/utils/store/Money.svelte';
	import { goto } from '$app/navigation';
	import { createCart } from '$src/lib/utils/shopify/cart';
	export let data;
	const { product } = data;
	const selectedVariant = product.variants.nodes[0];
	console.log(selectedVariant);
	async function buyNow() {
		if (!product) throw Error('Product not found');
		const cart = await createCart(selectedVariant.id, 1);
		if (!cart) throw Error('Cart creation failed');
		goto(cart.checkoutUrl);
	}
	//TODO: REMOVE THIS AFTER TESTING discount pricing
	selectedVariant.compareAtPrice.amount = 39.99;

	//This is for pixel alignments on the "updated " banner
	$: is_on_sale = selectedVariant?.compareAtPrice.amount != selectedVariant.price.amount;
</script>

<div
	class="z-10 absolute w-full md:w-auto right-0 bottom-0 md:right-[10vw] 3xl:right-[20vw] md:bottom-[10vh] md:skew-x-[25deg] bg-primary-side md:pl-4"
>
	<div
		class="flex flex-col bg-text-primary text-black md:[&>*]:-skew-x-[25deg] px-20 py-4 pb-8 gap-4"
	>
		<h1 class="before:content-[''] mx-auto md:-mb-3 mt-3">
			<span class="block text-4xl w-full -mb-2">HyPERTRANCE</span>
			<span class="pl-24 block text-4xl">SAMPLEPACK</span>
		</h1>

		<div>
			<div
				class="w-full uppercase [&>*]:skew-x-[25deg] mt-4 mb-1 text-lg font-bold font-michroma whitespace-nowrap"
			>
				<div
					class="w-fit bg-red-700 m-0 -translate-x-1/2 absolute left-[calc(50%-1.8rem)] -translate-y-8 md:left-0 md:translate-x-0 md:translate-y-0 md:relative md:m-0 {is_on_sale
						? `md:ml-0.5`
						: `md:ml-1`} {is_on_sale ? `px-[1.15rem]` : `px-[1.1rem]`}"
				>
					<span class="block -skew-x-[25deg] text-white"> updated november 2024 </span>
				</div>
			</div>
			<!-- {#if true || (selectedVariant.compareAtPrice !== null && selectedVariant.compareAtPrice?.amount !== selectedVariant.price?.amount)} -->
			<div class="flex justify-center text-2xl font-michroma -ml-3">
				<button
					class="hyper-button px-8 pt-3 button-primary-inverse skew-x-[25deg] [&>*]:-skew-x-[25deg]"
					on:click={buyNow}
				>
					<span class="inline-block">buy now</span>
				</button>

				<!--  -->
				<Money
					price={selectedVariant.price}
					showCurrency={true}
					discountPrice={is_on_sale ? selectedVariant?.compareAtPrice : undefined}
					class="flex items-center h-full px-10 py-4 skew-x-[25deg] [&>*]:-skew-x-[25deg] text-base  bg-primary-side text-primary ring-1 ring-primary"
				/>
			</div>
			<div class="-mb-4 text-center md:text-end text-lg">
				<a
					href="/store/products/hypertrance-samplepack"
					data-sveltekit-reload
					class="inline-block text-bg-primary hover:text-black active:text-black py-1 px-8 m-2 mt-0"
					>learn more</a
				>
			</div>
		</div>
	</div>
</div>
