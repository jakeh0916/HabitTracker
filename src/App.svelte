<script>
  import HabitCard from "./lib/HabitCard.svelte";
  import UserCard from "./lib/UserCard.svelte";

  // This class represents the User's basic data. This can be used to set up (or save/load)
  // existing data into the application.
  class UserData {
    constructor(name, startDate) {
      this.name = name;
      this.startDate = startDate;
    }
  }

  // This class represents an individual Habit's data. This can be used to set up (or save/load)
  // existing habit data into the application.
  //
  // BUG: There is currently a bug where adding a new habit will cause other habits to clear
  // their data. The fix is to pass in the HabitData object itself to the HabitCard component,
  // that way it can write its data directly to this object.
  class HabitData {
    constructor(title, description, type="binary", historyString="") {
      this.title = title;
      this.description = description;
      this.type = type;
      this.historyString = historyString;
      this.deleteCallback = () => { deleteHabit(this) }
    }
  }

  // Adds a new Habit Card to the UI.
  function addHabit(type) {
    const habit = new HabitData("", "", type, "");
    habitDatas.push(habit);
    habitDatas = habitDatas;
  }

  // Removes an existing Habit Card from the UI.
  function deleteHabit(habit) {
    const index = habitDatas.indexOf(habit);
    if (index >= 0)
    {
      habitDatas.splice(index, 1);
      habitDatas = habitDatas;
    }
  }

  // Current User Data.
  // (DEBUG: To load/demo a particular user, set it here.)
  let userData = new UserData("", ""); // Example Data: new UserData("Jake Huseman", "2024-09-20");

  // Current Habits.
  // (DEBUG: To load/demo some particular habits, set them here.)
  let habitDatas = [

    /* Example Data:

    new HabitData("Walk the dog", "walk the dog sometime after 2pm each day", "binary", "2024-09-20: Success\n2024-09-21: Success\n2024-09-22: Success\n"),
    new HabitData("Go to the gym", "need to workout every day. go to gym around 9am?", "binary", "2024-09-20: Success\n2024-09-21: Fail!\n2024-09-22: Success\n"),
    new HabitData("Drink 10 cups of water!", "gotta drink more water!", "number", "2024-09-20: 11 Cups\n2024-09-21: 10 Cups\n2024-09-22: 8 Cups (or so)\n"),
    new HabitData("Do homework", "someone's gotta do it", "binary", "2024-09-20: Success\n2024-09-21: Success\n2024-09-22: Success\n"),
    new HabitData("Check canvas", "check canvas after classes, before bed!", "binary", "2024-09-20: Success\n2024-09-21: Success\n2024-09-22: Success\n"),
    */

  ]

  // Night mode toggle. The application can be toggled between day (light) and night (dark) modes.
  let nightMode = true;

  // Toggles the current day/night mode.
  function toggleNightMode() {
    nightMode = !nightMode;

    if (nightMode) {
      document.documentElement.style.setProperty("--background-1", "#263844");
      document.documentElement.style.setProperty("--background-2", "#354B58");
      document.documentElement.style.setProperty("--foreground-1", "#ffffff");
      document.documentElement.style.setProperty("--foreground-2", "#cad1d5");
      document.documentElement.style.setProperty("--focus-good", "#88ab83");
      document.documentElement.style.setProperty("--focus-bad", "#ce8078");
    }
    else {
      document.documentElement.style.setProperty("--background-1", "#ffffff");
      document.documentElement.style.setProperty("--background-2", "#cad1d5");
      document.documentElement.style.setProperty("--foreground-1", "#263844");
      document.documentElement.style.setProperty("--foreground-2", "#354B58");
      document.documentElement.style.setProperty("--focus-good", "#a9f29e");
      document.documentElement.style.setProperty("--focus-bad", "#ffb9b3");
    }
  }

  // Current date. (Shown mainly for debug purposes.)
  const date = Date();
</script>

<main>
  <span id="app-title">Habit Tracker</span>
  <span id="app-description">Created by Jake Huseman</span>

  {#if nightMode}
    <button id="app-style-toggle-night" on:click={toggleNightMode}>Day Mode</button>
  {:else}
    <button id="app-style-toggle-day" on:click={toggleNightMode}>Night Mode</button>
  {/if}
  
  <span id="app-date">{date}</span>

  <div id="card-container">
    <UserCard name={userData.name} startDate={userData.startDate} addHabitCallback={addHabit}></UserCard>
    {#each habitDatas as habitData}
      <HabitCard title={habitData.title} description={habitData.description} type={habitData.type} historyString={habitData.historyString} deleteCallback={habitData.deleteCallback}></HabitCard>
    {/each}
  </div>
</main>
