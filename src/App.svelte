<script lang="ts">
  import Modal from './Modal.svelte'
  import AddPersonForm from './AddPersonForm.svelte'
  import PersonOuter from './PersonOuter.svelte'
  import CustomButton from './CustomButton.svelte'
  import axios from 'axios'
  import { onMount, beforeUpdate, afterUpdate, tick } from 'svelte'

  let count = 0
  $: double = count * 2
  let previousCount = 0
  let backgroundColor: string = 'red'
  let people: {
    name: string
    beltColor: string
    age: number
    id: number
    mood?: string
  }[] = [
    { name: 'Daniils', beltColor: 'yellow', age: 23, id: 2 },
    { name: 'Nikita', beltColor: 'green', age: 20, id: 1 },
    { name: 'Sam', beltColor: 'black', age: 26, id: 3 },
  ]
  const handleDelete = (e: any) => {
    console.log(e)
    const eventDetails = e.detail
    people = people.filter((person) => person.id !== eventDetails.id)
  }
  let showModal = false
  const toggleModal = () => {
    showModal = !showModal
  }

  const changeObjects = () => {
    const moods = ['depressed', 'happy', 'content', 'sporty']
    const newPeople = people.map((el) => ({
      ...el,
      mood: moods[Math.floor(Math.random() * moods.length - 1)],
    }))
    people = newPeople
  }

  const addPerson = (event) => {
    const person = event.detail
    people = [person, ...people]
    showModal = false
  }

  let obj: { userId: number; id: number; title: string; completed: boolean }

  async function testReq() {
    const res = await axios.get('https://jsonplaceholder.typicode.com/todos/1')
    const data = await res.data
    obj = data
    return data
  }
  async function onMountTestReq() {
    const res = await axios.get('https://jsonplaceholder.typicode.com/todos/1')
    const data = await res.data
    console.log('from onMount', data)
  }
  onMount(() => onMountTestReq())

  let personPromise = testReq()
  function handlePersonReq() {
    personPromise = testReq()
  }

  function handleDivClickPropagate(event) {
    console.log('Div clicked!')
  }
  function handleLinkClick(event) {
    event.preventDefault()
    event.stopPropagation()
    console.log('Link clicked!')
  }

  async function handleCustomButton(event) {
    console.log('before tick', double)
    await tick()
    count += 1
  }

  beforeUpdate(() => {
    if (count !== previousCount) {
      previousCount = count
      console.log('count on beforeUpdate', count)
    } else {
      // no change in count, so no need to update the DOM
      return false
    }
  })
  afterUpdate(() => {
    console.log('count on afterUpdate', count)
  })
</script>

<Modal {showModal} on:click={toggleModal}>
  <AddPersonForm on:addPerson={addPerson} />
</Modal>
<main>
  <!-- for each loop  -->
  <button on:click|once={toggleModal}>Open Modal</button>
  {#each people as person (person.id)}
    <PersonOuter {person} on:deletePerson={handleDelete} />
  {:else}
    <p>There are no people to show ...</p>
  {/each}
  <button on:click|once={changeObjects}>Add mood...</button>
</main>
<!-- Promises -->
<div class="promise">
  <button on:click={handlePersonReq}>Press to get a Person Data</button>
  {#await personPromise then response}
    <h2>Title: {response.title}</h2>
  {/await}
  {#if obj}
    <h1>{obj.title}</h1>
  {/if}
</div>
<!-- Event modifiers -->

<div
  on:click={handleDivClickPropagate}
  style="background-color: {backgroundColor}"
>
  <a href="https://svelte.dev/tutorial/basics" on:click={handleLinkClick}
    >Click on the link</a
  >
</div>

<!-- Custom Button Click -->
<CustomButton on:click={handleCustomButton} />
<h1>{double}</h1>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
  .promise {
    position: relative;
    left: 50%;
  }
</style>
