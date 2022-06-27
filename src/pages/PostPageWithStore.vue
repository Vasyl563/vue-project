<template>
<div>
    <!-- <h1>{{ $store.state.isAuth ? "Користувач авторизований" :  "Авторизуйтеь, щоб використовувати сервіс"}}</h1>
    <h1>{{ $store.getters.doubleLikes }}</h1>
    <div>
        <my-button @click="$store.commit('incrementLikes')">Like</my-button>
        <my-button @click="$store.commit('decrementLikes')">Dislike</my-button>
    </div> -->
    <h1>Сторінка з постами</h1>
    <my-input 
    v-focus
    :model-value="searchQuery"
    @update:model-value="setSearchQuery"
    placeholder="Пошук..."
    />
    <div class="app__btns">
   <my-button
   @click="showDialog"
   >
       Створити користувача
       </my-button>
      <my-select 
      :model-value="selectedSort"
      @update:model-value="setSelectedSort"
      :options="sortOptions"
      />
      </div>
    <my-dialog v-model:show="dialogVisible">
        <post-form 
       @create="createPost" 
       />
    </my-dialog>
      <post-list
        :posts="sortedAndSearchedPosts"
        @remove="removePost"
        v-if="!isPostsLoading"
    />
        <div v-else>Йде загрузка...</div>  
        <div v-intersection="loadMorePosts" class="observer"></div> 
         <div class="page__wrapper">
            <div v-for="pageNumber in totalPages" :key="pageNumber" class="page"
            :class="{
            'current-page': page === pageNumber
            }"
            @click="changePage(pageNumber)"
            >
            {{ pageNumber }}
        </div>
    </div>
</div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import MyButton from "@/components/UI/MyButton";
import axios from "axios";
import MySelect from "@/components/UI/MySelect";
import MyInput from "@/components/UI/MyInput"
import {mapState, mapGetters, mapActions, mapMutations} from 'vuex'
export default {
    components: {
        MyInput,
        MySelect,
        MyButton,  
        PostList,
        PostForm,
              

    },
    data() {
        return {
        //    posts: [ ],
           dialogVisible: false,
        //    isPostsLoading: false,
        //    selectedSort: '',
        //    searchQuery: '',
        //    page: 1,
        //    limit: 10,
        //    totalPages: 0,
        //    sortOptions: [
        //        {value: 'title', name: 'По назві'},
        //        {value: 'body', name: 'По содержимому'},
        //    ]
           
        }
    },
    methods: {
        ...mapMutations({
            setPage: 'post/setPage',
            setSearchQuery: 'post/setSearchQuery',
            setSelectedSort: 'post/setSelectedSort'
        }),
        ...mapActions({
            LoadMorePosts: 'post/LoadMorePosts',
            fetchPosts: 'post/fetchPosts'
        }),
      createPost(post) {
       this.posts.push(post); 
       this.dialogVisible = false;
      },
      removePost(post) {
          this.posts = this.posts.filter(p => p.id !== post.id)
      },
      showDialog() {
          this.dialogVisible = true;
      },
        // changePage(pageNumber) {
        //     this.page = pageNumber
            
        // },
//       async fetchPosts() {
//           try {
//               this.isPostsLoading = true;
              
//    const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
//        params: {
//            _page: this.page,
//            _limit: this.limit
//        }
//    });
//                 this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
//               this.posts = response.data;
//           } catch (e) {
//               alert('Err')
//           } finally {
//               this.isPostsLoading = false;
//           }
//       },
    },
     mounted() {
         this.fetchPosts();
        //   this.fetchPosts();
//           const options = {
//     rootMargin: '0px',
//     threshold: 1.0
// }
// const callback = (entries, observer) => {
//    if (entries[0].isIntersecting && this.page < this.totalPages) {
//        this.loadMorePosts()
//    }
// };
// const observer = new IntersectionObserver(callback, options);
// observer.observe(this.$refs.observer);
      },
        computed: {
            ...mapState({
                posts: state => state.post.posts,
                isPostsLoading: state => state.post.isPostsLoading,
                selectedSort: state => state.post.selectedSort,
                searchQuery: state => state.post.searchQuery,
                page: state => state.post.page,
                limit: state => state.post.limit,
                totalPages: state => state.post.totalPages,
                sortOptions: state => state.post.sortOptions,
            }),
            ...mapGetters({
                sortedPosts: 'post/sortedPosts',
                sortedAndSearchedPosts: 'post/sortedAndSearchedPosts'
            }),
            // sortedPosts() {
            //     return [... this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
            // },
            // sortedAndSearchedPosts() {
            //     return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
            // }
        },
      watch: {
        //   page() {
        //       this.fetchPosts()
        //   }
      }
}
</script>

<style>

.app__btns {
    margin: 15px 0;
    display: flex;
    justify-content: space-between;
}
.page__wrapper {
    display: flex;
    margin-top: 15px;
}
.page {
    border: 1px solid black;
    padding: 10px;
}
.current-page {
border: 2px solid teal;
}
.observer {
    height: 30px;
    background: green;
}
</style>


