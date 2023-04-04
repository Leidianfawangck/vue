<template>
    <div class="wallpaper">
        <div class="loading">
            加载中...
        </div>
    </div>
</template>
 
<script>
export default {
    name: 'Loading',
    props: {
    },
    data() {
        return {

        }
    },
}
</script>
<style>
.wallpaper {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: rgba(165, 158, 158, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
}

.loading {
    width: 300px;
    height: 100px;
    line-height: 100px;
    border: 1px solid #000;
    border-radius: 5px;
    background-color: #ccc;
}
</style>

