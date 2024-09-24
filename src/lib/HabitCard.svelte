<script>
    // Habit Card properties. These properties can be modified by the user
    // to customize the habit. Additionally, the history is updated each
    // time the user completes a habit for the day.
    export let title = "";
    export let description = "";
    export let type = "";
    export let historyString = "";
    export let deleteCallback = null;

    // (Bookkeeping variables.)
    let lastDateString = "";
    let habitInput = "";
    let updatedToday = false;

    function addToHistory(text) {
        updatedToday = true;

        let newEditDate = new Date();
        newEditDate.setHours(0, 0, 0, 0);

        let newDateString = newEditDate.toISOString();
        newDateString = newDateString.slice(0, 10);

        if (newDateString === lastDateString)
        {
            return
        }
    
        lastDateString = newDateString;
        historyString += lastDateString + ": " + text + "\n";
    }
</script>

<div class="card">
    <div style="display: flex">
        <input class="card-input" title="Edit Habit Title" style="flex-grow: 1; font-weight: 700" placeholder="Habit Title" bind:value={title} />
        <button class="card-input" title="Delete Habit" on:click={deleteCallback}>✖</button>
    </div>

    <div style="display: flex;">
        <textarea class="card-input" title="Edit Habit Description" style="flex-grow: 1;" placeholder="Habit Description" bind:value={description}></textarea>
    </div>

    {#if updatedToday}
        <div style="display: flex;">
            <textarea readonly class="card-input" style="flex-grow: 1; height: 208px;">History: {"\n\n"}{historyString}</textarea>
        </div>
    {:else}
        <div style="display: flex;">
            <textarea readonly class="card-input" style="flex-grow: 1; height: 140px;">History: {"\n\n"}{historyString}</textarea>
        </div>
        <div class="card-sep"></div>
        <div>
            {#if type === "binary"}
                <button class="card-input focus-good" title="'{title}' was completed today." style="width: 46.25%;" on:click={() => { addToHistory("Completed.") }}>Completed</button>
                <button class="card-input focus-bad" title="'{title}' was not completed today." style="width: 46.25%;" on:click={() => { addToHistory("Failed.") }}>Failed</button>
            {:else if type === "number"}
                <input type="number" class="card-input" title="'{title}' status for today." style="width: 92.5%;" placeholder="Today's status" bind:value={habitInput} on:change={() => { addToHistory(habitInput) }} />
            {:else if type === "text"}
                <input type="text" class="card-input" title="'{title}' status for today." style="width: 92.5%;" placeholder="Today's status" bind:value={habitInput} on:change={() => { addToHistory(habitInput) }} />
            {/if}
        </div>
    {/if}
</div>
