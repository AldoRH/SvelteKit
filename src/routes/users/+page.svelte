<script>
    import { users } from '$lib/users';
  
    let usersList = users;
  </script>
  
  <h1>Lista de Usuarios</h1>
  
  <ul>
    {#each usersList as user}
      <li>
        {user.user} <a href={`/users/${user.id}`}>Editar</a>
      </li>
    {/each}
  </ul>