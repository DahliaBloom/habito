<script>
    import Habit from "./Habit.svelte";
    import { habits } from "../utils/localStore";
</script>

<div class="flex flex-wrap gap-2 p-2 justify-center">
    {#each $habits as habit}
        <Habit {habit} />
    {:else}
        <!-- TODO-->
        No habits
    {/each}
</div>
