<template>
    <!--<h1></h1>-->
    <div>
        <h1>分类</h1>
        <scroll class="left">
            <h1>1</h1>
            <h1>2</h1>
            <h1>3</h1>
            <h1>4</h1>
            <h1>5</h1>
            <h1>6</h1>
            <h1>7</h1>
            <h1>8</h1>
            <h1>9</h1>
            <h1>10</h1>
        </scroll>
    </div>
</template>

<script>
    import Scroll from 'components/common/scroll/Scroll'

    export default {
        name: "Category",
        components:{
            Scroll
        }
    }
</script>

<style scoped>
    .left{
        height: 200px;
        overflow: hidden;
    }
</style>