<template>
<div class="post">
    <div>
        <div>{{ post.id }}</div>
    <div><strong>Назва</strong> {{ post.title }} </div>
  <div><strong>Опис</strong> {{ post.body }} </div> 
    </div>
    <div class="post__btns">

        <my-button
        @click="$router.push(`/posts/${post.id}`)"
        >
        Open 
        </my-button>
        <my-button
         @click="$emit('remove', post)"
        >
            Deleted
        </my-button>
    </div>
</div>  
</template>

<script>
export default {
    props: {
        post: {
            type: Object,
            required: true,
        }
    }
}
</script>

<style scoped>
.post {
    padding: 15px;
    border: 3px solid green;
    margin-top: 15px;
    display: flex;
    align-items: center;
    justify-content: space-between;
}
.post__btns {
    display: flex;
}
</style>