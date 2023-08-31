<script>
    export let habit;

    $: checkedDaysInMap = getCheckedInLast105Days(habit.checkedDays);

    function getCheckedInLast105Days(checkedDays) {
        const millisecondsPerDay = 24 * 60 * 60 * 1000;
        const currentDate = new Date();
        let currentDayOfWeek = currentDate.getDay() - 1;
        if (currentDayOfWeek == -1) currentDayOfWeek = 7;
        const daysUntilNextSunday = 7 - currentDayOfWeek;
        const nextSunday = new Date(
            currentDate.getFullYear(),
            currentDate.getMonth(),
            currentDate.getDate() + daysUntilNextSunday
        ).getTime();

        let acc = [];
        for (let i = checkedDays.length - 1; i >= 0; i--) {
            const daysPassed = Math.max(
                0,
                Math.floor((nextSunday - checkedDays[i]) / millisecondsPerDay)
            );

            if (daysPassed > 104) {
                break;
            }
            acc.push(104 - daysPassed);
        }

        return acc;
    }
</script>

<div class="grid grid-flow-col gap-1">
    {#each Array(105) as _, i}
        <div
            class="{checkedDaysInMap.includes(i)
                ? 'bg-primary'
                : 'bg-neutral-focus'} aspect-square w-2 sm:w-4 rounded-sm"
        />
    {/each}
</div>

<style>
    div {
        grid-template-rows: repeat(7, minmax(0, 1fr));
    }
</style>
