<template>
	<div class="cell" :class="{'cell-today': isToday,'cell-weekEnd': isWeekEnd,'cell-past': isPast}">
		<span class="cell__day" >{{date}}</span>
		<Badge
			v-for="event of eventsInThisDay"
			:key="event.id"
			:eventData="event"
		></Badge>
	</div>
</template>
<script>
	import Badge from "@/components/Badge"
	export default
	{
		props:
		{
			date:String,
			eventsInThisDay:Object,
			isToday:Boolean,
			isWeekEnd:Boolean,
			isPast:Boolean,
		},
		components: {Badge}
	}
</script>
<style lang="scss">
	.cell
	{
		position: relative;
		flex-direction: column;
		padding: 0 10px;
		padding-top: 35px;
		padding-bottom: 15px;
		width: 150px;
		height: 100px;
		border-radius: 7px;
		border: 1px solid #F8F8F8;
		font-family: 'Roboto';
		&:hover{border: 1px solid #B8BAB9;}
	}
	.cell-today .cell__day{ color: #95BCA1;}
	.cell__day
	{
		position:absolute;
		right: 10px;
		top: 5px;
		font-size:21px;
		font-weight:500;
	}
	.cell-past
	{
		background: #F5F5F5;
		.cell__day{color:#A1A1A1;}
	}
	.cell-weekEnd .cell__day{color:#919cd8;}
</style>