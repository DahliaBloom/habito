<script>
    import HeatMap from "./HeatMap.svelte";
    import { Check } from "lucide-svelte";
    import { habits } from "../utils/localStore";
    import { Confetti } from "svelte-confetti";
    import HabitMenue from "./HabitMenue.svelte";

    export let habit;

    let playConfetti = false;
    let isOpenHabitMenue = false;

    $: isCheckedForToday =
        habit.checkedDays.length !== 0 &&
        isSameDay(
            new Date(habit.checkedDays[habit.checkedDays.length - 1]),
            new Date()
        );

    function isSameDay(date1, date2) {
        return (
            date1.getFullYear() === date2.getFullYear() &&
            date1.getMonth() === date2.getMonth() &&
            date1.getDate() === date2.getDate()
        );
    }
</script>

<button
    class="bg-neutral rounded-lg p-2"
    on:click={() => (isOpenHabitMenue = true)}
>
    <div class="mb-2 flex justify-between">
        <h1 class="font-bold text-base sm:text-xl">{habit.name}</h1>

        <div class="relative">
            <button
                class="btn-circle {isCheckedForToday
                    ? 'bg-success'
                    : 'bg-neutral-focus'} h-fit w-fit p-1"
                on:click={() => {
                    if (!isCheckedForToday) {
                        playConfetti = true;
                        habit.checkedDays.push(Date.now());
                        habits.update((v) => v);
                    }
                }}
            >
                <Check size="13" class="stroke-white" />
            </button>
            {#if playConfetti}
                <div class="absolute top-1/2 left-1/2">
                    <Confetti y={[-0.3, 0.3]} x={[-0.3, 0.3]} />
                </div>
            {/if}
        </div>
    </div>
    <HeatMap {habit} />
</button>

{#if isOpenHabitMenue}
    <HabitMenue
        {habit}
        on:close={() => {
            isOpenHabitMenue = false;
            habits.update((v) => v.filter((obj) => obj.name !== habit.name));
        }}
    />
{/if}
