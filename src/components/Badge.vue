<template>
	<p class="Bagde" :class="'Bagde__' + eventData.type">
		{{eventData.name}}
	</p>
</template>
<script>
	export default
	{
		props:{eventData:Object}
	}
</script>
<style lang="scss">
	.Bagde
	{
		position: relative;
		z-index: 1;
		margin: 2px 0;
		padding:0 5px;
		border-radius: 4px;
		white-space: nowrap;
		overflow: hidden;
		overflow-x: auto;
		text-overflow: ellipsis;
		font-size: 21px;
		font-weight: 400;
		&:hover
		{
			position: relative;
			z-index: 1;
			overflow: visible;
			min-width: max-content;
		}
	}
	.Bagde__orange
	{
		background: #F8EBD8;
		color:#D3AB73;
	}
	.Bagde__red
	{
		background: #FCECEF;
		color:#C67F86;
	}
	.Bagde__green
	{
		background: #E9F9EF;
		color:#A3CFAC;
	}
</style>