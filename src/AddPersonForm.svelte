<script>
  import { createEventDispatcher } from 'svelte'
  import { onDestroy } from 'svelte'
  import { onMount } from 'svelte'

  onMount(() => console.log('The modal has been mounted'))
  onDestroy(() => {
    console.log('The modal has been destroyed')
  })



  let dispatch = createEventDispatcher()

  let name
  let beltColor
  let age
  const handleSubmit = () => {
    const person = {
      name: name,
      beltColor: beltColor,
      age: age,
      skills: skills,
      id: Math.random(),
    }
    dispatch('addPerson', person)
  }
  let skills = []
  // let fighting =false
  // let sneaking = false
  // let running = false
</script>

<form on:submit|preventDefault={handleSubmit}>
  <input type="text" placeholder="name" bind:value={name} />

  <input type="number" placeholder="age" bind:value={age} />
  <!-- svelte-ignore a11y-label-has-associated-control -->
  <label>Skills:</label>

  <input type="checkbox" bind:group={skills} value="fighting" /> Fighting <br />
  <input type="checkbox" bind:group={skills} value="sneaking" /> Sneaking <br />
  <input type="checkbox" bind:group={skills} value="running" /> Running <br />
  <label for="check">Belt Color</label>
  <select bind:value={beltColor} id="check">
    <option value="black">black</option>
    <option value="orange">orange</option>
    <option value="brown">brown</option>
    <option value="purple">purple</option>
  </select>
  <button>Add Person</button>
</form>

<style></style>
