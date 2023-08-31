<script>
    import { Plus } from "lucide-svelte";
    import AddHabit from "./AddHabit.svelte";

    let addingHabit = false;
</script>

<button
    class="btn-circle bg-secondary w-fit h-fit p-1 hover:scale-95"
    on:click={() => (addingHabit = true)}
>
    <Plus class="w-4 h-4 sm:w-6 sm:h-6" />
</button>

{#if addingHabit}
    <AddHabit on:close={() => (addingHabit = false)} />
{/if}
