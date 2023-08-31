<script>
    import { onMount } from "svelte";
    import { createEventDispatcher } from "svelte";
    import { habits } from "../utils/localStore";
    import { Trash2 } from "lucide-svelte";

    const dispatch = createEventDispatcher();

    export let habit;

    let dialog;

    onMount(() => {
        dialog.onclose = () => dispatch("close");
        dialog.showModal();
    });

    function del() {
        habits.update((v) => v.filter((obj) => obj.name !== habit.name));
    }
</script>

<div style="all: unset;">
    <dialog class="modal modal-bottom sm:modal-middle" bind:this={dialog}>
        <form method="dialog" class="modal-box">
            <!-- TODO: validate -->
            <button
                class="btn btn-sm btn-circle btn-ghost absolute right-4 top-4"
                >✕</button
            >
            <h3 class="font-bold text-lg">{habit.name}</h3>
            <div class="mt-4 flex flex-col gap-2">
                <button
                    on:click={del}
                    type="button"
                    class="btn text-white normal-case w-full btn-error"
                    ><span class="flex items-center gap-2">
                        <span class="text-base">Delete habit</span>
                        <Trash2 />
                    </span>
                </button>
            </div>
        </form>
        <form method="dialog" class="modal-backdrop">
            <button />
        </form>
    </dialog>
</div>
