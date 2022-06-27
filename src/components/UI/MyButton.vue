<template>
    <button class="btn">
        <slot></slot>
    </button>
</template>

<script>
export default {
    name: 'my-button'
}
</script>


<style scoped>
.btn {
    padding: 10px 15px;
    color: rgb(7, 167, 87);
    background: none;
    border: 1px solid rgb(7, 167, 87);
}
</style>
