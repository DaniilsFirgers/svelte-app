<script lang="ts">
  export let person: {
    name: string
    beltColor: string
    age: number
    id: number
    mood?: string
  }
  import PersonInner from './PersonInner.svelte'
</script>

<PersonInner {person} on:deletePerson />
