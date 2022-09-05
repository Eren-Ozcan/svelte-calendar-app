<script>
	export let schedule;
	const date = new Date();
	
	const today = {
		dayNumber: date.getDate(),
		month: date.getMonth(),
		year: date.getFullYear(),
	}
	
	const getNextMonth = (/** @type {number} */ i) => {
		if (i >= 11) {
			return 0;
		}
		return i + 1;
	}

	const getPrevMonth = (/** @type {number} */ i) => {
		if (i <= 0) {
			return 11;
		}
		return i - 1;
	}
	
	const monthNames = [ "Ocak", "Şubat", "Mart", "Nisan", "Mayıs", "Haziran", "Temmuz", "Ağustos", "Eylül", "Ekim", "Kasım", "Aralık"];
	let monthIndex = date.getMonth();
	$: prevMonthIndex= getPrevMonth(monthIndex);
	$: nextMonthIndex= getNextMonth(monthIndex);
	// const currentMonth = date.toLocaleString('en-US', { month: 'long' })
	let year = date.getFullYear();

	$: month = monthNames[monthIndex];
	$: firstDayIndex = new Date(year, monthIndex, 0).getDay();
	$: numberOfDays = new Date(year, monthIndex+1, 0).getDate();

	$: calendarCellsQty = firstDayIndex <= 4 ? 35 : 42;	
	

	$: prevMonth = monthNames[prevMonthIndex];
	$: prevFirstDayIndex = new Date(prevMonthIndex==11 ? year-1:year, prevMonthIndex, 0).getDay();
	$: prevNumberOfDays = new Date(prevMonthIndex==11 ? year-1:year, prevMonthIndex+1, 0).getDate();

	$: nextMonth = monthNames[nextMonthIndex];
	$: nextFirstDayIndex = new Date(nextMonthIndex==0 ? year+1:year, nextMonthIndex, 0).getDay();
	$: nextNumberOfDays = new Date(nextMonthIndex==0 ? year+1:year, nextMonthIndex+1, 0).getDate();

	const goToNextMonth = () => {
		if (monthIndex >= 11) {
			year += 1;
			return monthIndex = 0;
		}
		return monthIndex += 1;
	}
	
	const goToPrevMonth = () => {
		if (monthIndex <= 0) {
			year -= 1;
			return monthIndex = 11;
		}
		return monthIndex -= 1;
	}
	
// 	$: console.log(`${month}, ${today.dayNumber}, ${year}, FIRST DAY index is ${firstDayIndex}, MONTH index is ${monthIndex}, No. of days: ${numberOfDays}`)
</script>


	<div class="month">
		<ul>
			<li class="prev" on:click={goToPrevMonth}>&#10094;</li>
			<li class="next" on:click={goToNextMonth}>&#10095;</li>
			<li>{month}<br>
				<span style="font-size:18px">{year}</span>
			</li>
		</ul>
	</div>

	<ul class="weekdays">
		<li>Pt</li>
		<li>Sa</li>
		<li>Ça</li>
		<li>Pe</li>
		<li>Cu</li>
		<li>Ct</li>
		<li>Pa</li>
	</ul>

	<ul class="days">
		{#each Array(calendarCellsQty) as _, i}
			{#if i < firstDayIndex || i >= numberOfDays+firstDayIndex  }
			{#if i < firstDayIndex}
			<li class:active={false}
				data-dateID={`${prevMonth}_${prevNumberOfDays+i-firstDayIndex+1}_${prevMonthIndex==11 ? year-1:year}`}
				class:has-appts={`${prevMonth}_${prevNumberOfDays+i-firstDayIndex+1}_${prevMonthIndex==11 ? year-1:year}` in schedule}
				on:click>
				{prevNumberOfDays+i-firstDayIndex+1}
				</li>
			{:else}
	<li class:active={false}
				data-dateID={`${nextMonth}_${ i-numberOfDays-firstDayIndex+1}_${nextMonthIndex==0 ? year+1:year}`}
				class:has-appts={`${nextMonth}_${ i-numberOfDays-firstDayIndex+1}_${nextMonthIndex==0 ? year+1:year}` in schedule}
				on:click>
				{i-numberOfDays-firstDayIndex+1}
				</li>
			{/if}
			
			{:else}
				<li class:active={i === today.dayNumber+(firstDayIndex-1) &&
													monthIndex === today.month &&
													year === today.year}
						data-dateID={`${month}_${(i-firstDayIndex)+1}_${year}`}
						class:has-appts={`${month}_${(i-firstDayIndex)+1}_${year}` in schedule}
						on:click>
					{(i - firstDayIndex) + 1}
				</li>
			{/if}
		{/each}
	</ul>
			
<style>
	ul {list-style-type: none;}	

	/* Month header */
	.month {
		padding: 34px 15px;
		width: auto;
		background: #1abc9c;
		text-align: center;
	}

	/* Month list */
	.month ul {
		margin: 0;
		padding: 0;
	}

	

	.month ul li {
		color: white;
		font-size: 40px;
		text-transform: uppercase;
		letter-spacing: 3px;
	}

	/* Previous button inside month header */
	.month .prev {
		float: left;
		padding-top: 10px;
		cursor: pointer;
	}

	/* Next button */
	.month .next {
		float: right;
		padding-top: 10px;
		cursor: pointer;
	}

	/* Weekdays (Mon-Sun) */
	.weekdays {
		font-size: 20px;
		margin: 0;
		padding: 0px 0;
		letter-spacing: 2px;
		background-color:#ddd;
	}

	.weekdays li {
		display: inline-block;
		padding-bottom: 20px;
		padding-top: 20px;
		width: 13.6%;
		color: #808080;
		text-align: center;
	}

	/* Days (1-31) */
	.days {
		padding: 0px 0;
		background: #eee;
		margin: 0;
	}

	.days li {
		list-style-type: none;
		display: inline-block;
		border: 1px solid black;
		padding-bottom: 25px;
		padding-top: 25px;
		width: 14%;
		text-align: center;
		margin-bottom: 1px;
		font-size: 2.2rem;
		color: #808080;
		cursor: pointer;
		top:0px;
	}

	/* Highlight the "current" day */
	.active {
		background: #1abc9c;
		color: white;
	}
	
	.days li.has-appts {
		color: #F2480A;
	}
</style>