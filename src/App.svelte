<script lang="ts">
	import Modal from './Modal.svelte'
	import AddPersonForm from './AddPersonForm.svelte';
	let people = [
		{name: 'Daniils', beltColor: 'yellow', age: 23, id: 2},
		 {name: 'Nikita', beltColor: 'green', age: 20, id: 1}, 
		 {name: 'Sam', beltColor: 'black', age: 26, id: 3}
		 ]
    const handleClick=(e:any, id:number)=>{
		people = people.filter((person)=>person.id !== id)
	}
	let showModal = false
	const toggleModal=()=>{
		showModal = !showModal
	}

	const addPerson=(event)=>{
		const person = event.detail
		people=[person, ...people]
		showModal = false
	}
</script>
<Modal  showModal={showModal} on:click={toggleModal}>
	<AddPersonForm on:addPerson={addPerson}/>
</Modal>
<main>
	<!-- for each loop  -->
<button on:click|once={toggleModal}>Open Modal</button>
{#each people as person (person.id)}
	<div>
		<h4>{person.name}</h4>
		{#if person.beltColor === 'black'}
			<h1>Master Ninja</h1>
		{/if}
		<p>{person.age} years old, {person.beltColor} belt</p>
		<button on:click={(e)=>
			handleClick(e, person.id)
		}>delete</button>
	</div>
	{:else}
	<p>There are no people to show ...</p>
{/each}

</main>

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
</style>