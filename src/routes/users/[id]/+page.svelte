<script lang="ts">
  import { users, type User } from '$lib/users';
  import { page } from '$app/stores';
  import { browser } from '$app/environment';
    import { get } from 'svelte/store';

  let user: User | undefined;
  let editedName = '';
  let id: number;

  $: id = parseInt(get(page).params.id);

  $: {
    user = users.find(u => u.id === id);
    if (user) {
      editedName = user.user;
    }
  }

  function handleInput(event: Event) {
    const input = event.target as HTMLInputElement;
    editedName = input.value;
    console.log('Input changed:', editedName);
  }

  function saveUser() {
    if (user) {
      user.user = editedName;
      console.log('Saving User:', { ...user, user: editedName });
      if (browser) {
        import('$app/navigation').then(({ goto }) => {
          goto('/users');
        });
      }
    }
  }
</script>

<h1>Editar Usuario</h1>

{#if user}
  <form on:submit|preventDefault={saveUser}>
    <label for="user">Nombre del usuario</label>
    <input id="user" value={editedName} on:input={handleInput} />

    <button type="submit">Guardar</button>
  </form>
{:else}
  <p>Usuario no encontrado</p>
{/if}
