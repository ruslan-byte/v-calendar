<template>
	<div class="calendar">
		<div class="calendar__header">
			<button @click="changeMouthToPrev">
				<svg width="6" height="11" viewBox="0 0 6 11" fill="none" xmlns="http://www.w3.org/2000/svg">
					<path fill-rule="evenodd" clip-rule="evenodd" d="M5.70697 1.23271C5.89444 1.41408 5.99976 1.66004 5.99976 1.9165C5.99976 2.17297 5.89444 2.41893 5.70697 2.6003L2.41397 5.78523L5.70697 8.97016C5.88913 9.15257 5.98992 9.39688 5.98764 9.65048C5.98537 9.90407 5.8802 10.1466 5.69479 10.326C5.50938 10.5053 5.25857 10.607 4.99637 10.6092C4.73417 10.6114 4.48157 10.5139 4.29297 10.3378L0.29297 6.46903C0.105499 6.28765 0.000183105 6.04169 0.000183105 5.78523C0.000183105 5.52877 0.105499 5.28281 0.29297 5.10143L4.29297 1.23271C4.4805 1.05139 4.73481 0.94953 4.99997 0.94953C5.26513 0.94953 5.51944 1.05139 5.70697 1.23271Z" fill="black"/>
				</svg>
			</button>
			<p>{{takeActiveDate()}}</p>
			<button @click="changeMouthToNext">
				<svg width="6" height="11" viewBox="0 0 6 11" fill="none" xmlns="http://www.w3.org/2000/svg">
					<path fill-rule="evenodd" clip-rule="evenodd" d="M0.293031 10.3377C0.10556 10.1564 0.000244141 9.91039 0.000244141 9.65393C0.000244141 9.39747 0.10556 9.15151 0.293031 8.97013L3.58603 5.7852L0.293031 2.60028C0.110873 2.41786 0.0100779 2.17355 0.0123563 1.91996C0.0146347 1.66637 0.119804 1.42379 0.305212 1.24446C0.49062 1.06514 0.741433 0.963422 1.00363 0.961218C1.26583 0.959015 1.51843 1.0565 1.70703 1.23268L5.70703 5.10141C5.8945 5.28278 5.99982 5.52874 5.99982 5.7852C5.99982 6.04167 5.8945 6.28763 5.70703 6.469L1.70703 10.3377C1.5195 10.519 1.26519 10.6209 1.00003 10.6209C0.734866 10.6209 0.480558 10.519 0.293031 10.3377Z" fill="black"/>
				</svg>
			</button>
		</div>
		<div class="calendar__content">
			<h3 class="calendar__day-title" v-for="(dayTitle,index) in daysTitle" :key="index">{{dayTitle}}</h3>
			<div v-for="(day, index) in days" :key="index">
				<Cell
					:date="day.format('D')"
					:eventsInThisDay="events.filter(event => event.date === day.format('DD-MM-YYYY'))"
					:isToday="checkingForToDay(day)"
					:isWeekEnd="checkingForWeekEnd(day)"
					:isPast="checkingForPastDay(day)"
				/>
			</div>
		</div>
	</div>
</template>
<script>
	import moment from 'moment'
	import Cell from "@/components/Cell"
	import {ref,onMounted} from "vue"
	export default
	{
		props:{events:Array},
		components:{Cell},
		setup()
		{
			let days = ref([]),
				daysTitle = ['Понидельник','Вторник','Среда','Четверг','Пятница','Суббота','Воскресение'],
				mouthActive = 0;
			onMounted(() => {
				setMouth();
			});
			function setMouth()
			{
				let monthDate = moment().add(mouthActive, 'month').startOf('month');
				days.value = [...Array(monthDate.daysInMonth())].map((_, i) => {
					return monthDate.clone().add(i, 'day')
				});

				let lastDayInPreviousMouth = moment().add(mouthActive, 'month').subtract(1, 'months').endOf('month'),
					firstDayInNextMouth = moment().add(mouthActive + 1, 'months').startOf('month');
				while(days.value[0].days() != 1)
				{
					days.value.unshift(moment(lastDayInPreviousMouth));
					lastDayInPreviousMouth.subtract(1,'days')
				}
				while(days.value.length % 7 != 0)
				{
					days.value.push(moment(firstDayInNextMouth));
					firstDayInNextMouth.add(1,'days')
				}
			}
			function checkingForToDay(day){
				return moment().format('DD-MM-YYYY') == day.format('DD-MM-YYYY');
			}
			function checkingForWeekEnd(day)
			{
				return (day.days() == 6 || day.days() == 0)
			}
			function checkingForPastDay(day)
			{
				return (day < moment().subtract(1, 'day'))
			}
			function changeMouthToNext()
			{
				mouthActive++;
				setMouth();
			}
			function changeMouthToPrev()
			{
				mouthActive--;
				setMouth();
			}
			function takeActiveDate()
			{
				let activeYearIsTodayYear = moment().add(mouthActive, 'month').format('YYYY') == moment().format('YYYY');
				if( activeYearIsTodayYear )
				{
					return moment().add(mouthActive, 'month').locale('ru').format('MMMM');
				}
				else
				{
					return moment().add(mouthActive, 'month').locale('ru').format('MMMM') +' '+moment().add(mouthActive, 'month').format('YYYY');
				}
			}
			return {days, checkingForToDay, checkingForWeekEnd, checkingForPastDay, daysTitle, changeMouthToNext, changeMouthToPrev, takeActiveDate}
		},
	};
</script>
<style lang="scss">
.calendar__content
{
	display: grid;
	grid-gap:10px;
	grid-template-columns: repeat(7, 170px);
	grid-template-rows: 40px repeat(5, auto) ;
}
.calendar__day-title
{
	font-family: Roboto;
	text-transform: uppercase;
	font-size: 16px;
	text-align: right;
	&:nth-of-type(6),
	&:nth-of-type(7)
	{
		color:#A3A3A3;
	}
}
.calendar__header
{
	font-family: Roboto;
	font-weight: 500;
	text-transform: uppercase;
	font-size: 16px;
	color:#2E3358;
	button
	{
		background: transparent;
		border: none;
		cursor: pointer;
	}
	p
	{
		display: inline-block;
		min-width: 150px;
		text-align: center;
	}
}
</style>