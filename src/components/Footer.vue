<template>
    <footer>
        <p>All rights reserved by &copy; Vin</p>
    </footer>
    <router-view to="/clickme">CLICK4ME</router-view>
</template>


<script>
    export default {
        name: 'Footer'
    }

</script>


<style scoped>
    a {
        color: #333
    }

    footer {
        margin-top: 30px;
        text-align: center;
        font-size: 1em;
        color: steelblue;
    }
</style>