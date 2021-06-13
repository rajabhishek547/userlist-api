<template>
  <table class="table">
    <thead class="thead bg-dark text-white">
      <tr>
        <th scope="col">ID</th>
        <th scope="col">Email</th>
        <th scope="col">First Name</th>
        <th scope="col">Last Name</th>
        <th scope="col"> Avatar</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="repo in showRepos" :key="repo.id">
        <td>
          {{ repo.id }}
        </td>
        <td>
          {{ repo.email }}
        </td>
        <td>
          {{ repo.first_name }}
        </td>
        <td>
          {{ repo.last_name }}
        </td>
          <td>
                <img v-if="repo.avatar" class="avatar" :src="repo.avatar"/>
            </td>
      </tr>
    </tbody>
  </table>
</template>
<script>
export default {
  name: 'DataTable',
  props: {
    showRepos: Array
  }
}
</script>
<style scoped>
  .avatar {
    max-width: 50px;
  }
</style>