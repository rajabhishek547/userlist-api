<template>
    <div id="app" class="container">
        <DataTable :showRepos="showRepos" :repositories="repositories"/>
        <div v-if="loading" class="justify-content-center">
            <icons :icon="['fas', 'spinner']" class="fa-spinner"/>
        </div>
        <div class="my-4">
            <ul class="pagination pagination-md justify-content-center text-center">
                <li class="page-item" :class="page === 1 ? 'disabled' : ''">
                    <a
                            class="page-link"
                            @click="prevPage"
                    >
                        &laquo;
                    </a>
                </li>
                <li class="page-link" style="background-color: inherit">
                    {{ page }} of {{ lastPage }}
                </li>
                <li class="page-item" :class="page === lastPage ? 'disabled' : ''">
                    <a class="page-link"
                       @click="nextPage"
                    >
                        &raquo;
                    </a>
                </li>
            </ul>
        </div><!--./Pagination -->
    </div>
</template>
<script>
  import DataTable from './components/DataTable.vue';
  import axios from 'axios';
  const githubAPI = 'https://reqres.in/api/users?page=';
  export default {
    name: 'app',
    components: {
      DataTable
    },
    data() {
      return {
        pageNum: 1,
        repositories: [],
        page: 1,
        loading: false,
        perPage: 6,
        total:''
      }
    },
    mounted() {
      this.fetchData()
    },
    computed: {
      showRepos() {
        let start = (this.page-1) * this.perPage;
        console.log(start)
        let end = start + this.perPage;
        return this.repositories.slice(start, end);
      },
      lastPage() {
        let length = this.total;
        return length/ this.perPage;
      }
    },
    methods: {
      fetchData() {
        this.loading = true;
        axios.get(githubAPI + this.pageNum)
          .then(({data}) => {
            this.repositories = this.repositories.concat(data.data);
            this.loading = false;
            this.total=data.total
          })
          .catch((err) => {
            throw err;
          })
      },
      prevPage() {
        this.page--;
        window.scrollTo({top: 0, behavior: 'smooth'});
      },
      nextPage() {
        if (this.page == this.lastPage - 1) {
          this.pageNum++;
          this.fetchData();
        }
        this.page++;
        window.scrollTo({top: 0, behavior: 'smooth'});
      }
    }
  }
</script>
<style>
    @keyframes spinner {
        to {
            transform: rotate(360deg);
        }
    }
    .fa-spinner {
        animation: spinner 1s linear infinite;
    }
    a:hover {
        cursor: pointer;
    }
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>