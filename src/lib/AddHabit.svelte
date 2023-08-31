<script>
    import { onMount } from "svelte";
    import { createEventDispatcher } from "svelte";
    import { habits } from "../utils/localStore";

    const dispatch = createEventDispatcher();

    let dialog;

    onMount(() => {
        dialog.onclose = () => dispatch("close");
        dialog.showModal();
    });

    function createHabit(e) {
        if (e.submitter.type === "submit") {
            const data = new FormData(e.target);

            habits.update((v) => [
                ...v,
                {
                    name: data.get("name"),
                    checkedDays: [],
                },
            ]);
        }
    }
</script>

<div style="all: unset;">
    <dialog class="modal modal-bottom sm:modal-middle" bind:this={dialog}>
        <form method="dialog" class="modal-box" on:submit={createHabit}>
            <!-- TODO: validate -->
            <button
                class="btn btn-sm btn-circle btn-ghost absolute right-4 top-4"
                >✕</button
            >
            <h3 class="font-bold text-lg">Create a new habit 😁</h3>
            <div class="mt-4 flex flex-col gap-2">
                <input
                    type="text"
                    placeholder="Name"
                    name="name"
                    class="input input-bordered focus:outline-none focus:border-secondary input-primary w-full"
                />
                <button type="submit" class="btn w-full btn-outline btn-primary"
                    >Create habit 👊</button
                >
            </div>
        </form>
        <form method="dialog" class="modal-backdrop">
            <button />
        </form>
    </dialog>
</div>
