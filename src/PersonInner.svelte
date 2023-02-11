<script lang="ts">
  import { createEventDispatcher } from 'svelte'
  let dispatch = createEventDispatcher()
  export let person: {
    name: string
    beltColor: string
    age: number
    id: number
    mood?: string
  }

  const handleDelete = (e: Event, id: number) => {
    const deleteEvent = {
      event: e,
      id: id,
    }
    dispatch('deletePerson', deleteEvent)
  }
</script>

<div>
  <h4>{person.name}</h4>
  {#if person.beltColor === 'black'}
    <h1>Master Ninja</h1>
  {/if}
  <p>
    {person.age} years old, {person.beltColor} belt {#if person.mood !== undefined},
      mood - {person.mood}{/if}
  </p>
  <button on:click={(e) => handleDelete(e, person.id)}>delete</button>
</div>

<style></style>
