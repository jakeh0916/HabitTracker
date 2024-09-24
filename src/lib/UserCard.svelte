<script>
    // User data. The user can update their name in the system. The system will
    // keep track of how long the user has been using the app to track habits.
    export let name = "";
    export let startDate = "";
    export let addHabitCallback = (type) => {};

    let description = getDescription();

    // Gets the updated description (i.e. the text that shows how long you've been
    // tracking habits, etc.)
    function getDescription() {
        if (startDate === "" || name === "")
            return "";
    
        let currentDateObject = new Date(); 
        let startDateObject = new Date(startDate);

        let timeActive = startDateObject.getTime() - currentDateObject.getTime();
        let daysActive = Math.abs( Math.round( timeActive / (1000 * 60 * 60 * 24) ))

        return "Tracking habits for " + name + ".\n\nStarted tracking on " + startDate + ", which was around " + daysActive + " days ago."
    }

    // Updates the description in the UI.
    function updateDescription() {
        if (name === "") {
            return;
        }
        
        let newDate = new Date();
        newDate.setHours(0, 0, 0, 0);

        if (startDate === "") {
            startDate = newDate.toISOString();
            startDate = startDate.slice(0, 10);
        }

        description = getDescription();
    }
</script>

<div class="card">
    <div style="display: flex">
        <input class="card-input" title="Edit User Name" style="flex-grow: 1; font-weight: 700" placeholder="User's Name" bind:value={name} on:change={updateDescription}/>
    </div>

    <div style="display: flex;">
        <textarea readonly class="card-input" style="flex-grow: 1; height: 94px;" placeholder="(Enter your name to start tracking habits.)">{description}</textarea>
    </div>

    <div class="card-sep" style="margin-top: 25px; margin-bottom: 25px"></div>

    <button class="card-input" title="Start Tracking Habits!" style="width: 292px; font-weight: 700" on:click={()=>{ if (addHabitCallback) addHabitCallback("binary"); }}>Create Habit (Binary)</button>
    <button class="card-input" title="Start Tracking Habits!" style="width: 292px; font-weight: 700" on:click={()=>{ if (addHabitCallback) addHabitCallback("number"); }}>Create Habit (Number)</button>
    <button class="card-input" title="Start Tracking Habits!" style="width: 292px; font-weight: 700" on:click={()=>{ if (addHabitCallback) addHabitCallback("text"); }}>Create Habit (Text)</button>
</div>