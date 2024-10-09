<template>
    <div id="main-banner">
        <h1>Task Lobs</h1>
    </div>
</template>

<script>
    export default {
        name: "Banner"
    }
</script>

<style scoped>
#main-banner {
    background: #171A21;
    background-position: 0 -250px;
    background-size: cover;
    height: 200px;
    display: flex;
    align-items: center; /* Centraliza verticalmente */
    justify-content: center; /* Centraliza horizontalmente */
}

#main-banner h1 {
    color: #fff;
    text-align: center;
    font-size: 60px;
    padding: 20px 40px;
}
</style>