<template>
  <div>
    <h1>Страница с постами</h1>
    <my-input v-model="searchQuery" v-focus placeholder="Поиск..."/>
    <div class="app__btns">
      <my-button @click="showDialog">Создать пост</my-button>
      <my-select v-model="selectedSort" :options="sortOptions"/>
    </div>
    <my-dialog v-model:show="dialogVisible">
      <post-form @create="createPost"/>
    </my-dialog>
    <post-list :posts="sortedAndSearchedPosts" @remove="removePost" v-if="!isPostsLoading"/>
    <div v-else>
      Идет загрузка...
    </div>
    <div v-intersection="loadMorePost" class="observed">

    </div>
    <!--    <div class="page__wrapper">
          <div v-for="pageNumber in totalPage" :key="page" class="page" :class="{
            'current__page': page === pageNumber
          }" @click="changePage(pageNumber)">
            {{ pageNumber }}
          </div>
        </div>-->
  </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import axios from "axios";

export default {
  components: {
    PostList,
    PostForm,
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostsLoading: false,
      selectedSort: "",
      searchQuery: "",
      page: 1,
      limit: 10,
      totalPage: 0,
      sortOptions: [
        {value: 'title', name: 'По названию'},
        {value: 'body', name: 'По содержимому'},
      ]
    }
  },
  methods: {
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id);
    },
    showDialog() {
      this.dialogVisible = true;
    },
    /*    changePage(pageNumber) {
          this.page = pageNumber;
        },*/
    async fetchPosts() {
      try {
        this.isPostsLoading = true;
        const response = await axios.get('http://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        });
        this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = response.data;
      } catch (e) {
        alert(e);
      } finally {
        this.isPostsLoading = false;
      }
    },
    async loadMorePost() {
      try {
        this.page += 1;
        const response = await axios.get('http://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        });
        this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = [...this.posts, ...response.data];
      } catch (e) {
        alert(e);
      }
    },
  },
  mounted() {
    this.fetchPosts();

  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    }
  },
  watch: {
    /*page() {
      this.fetchPosts();
    }*/
  }
}
</script>

<style scoped>
.app__btns {

}
</style>