<template>

    <nav>
        <RouterLink to="/" id="logo_url">
            <img :src="src_img" :alt="alt_img" id="logo"/>
        </RouterLink>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/pedidos">Pedidos</RouterLink>
    </nav>

</template>

<script>
export default {
name: 'Navbar',
props:{
    src_img: String,
    alt_img: String
}
}
</script>

<style scoped>
nav{
    background: #222;
    border-bottom: 4px solid #111;
    padding: 15px 50px;
    display: flex;
    justify-content: flex-end;
    align-items: center;
}
#logo_url{
    margin:auto;
    margin-left: 0;
}
#logo{
    width: 40px;
    height: 40px;
}
nav a{
    color: #FCBA03;
    text-decoration: none;
    margin: 12px;
    transition: 0.5s;
}
nav a:hover{
    color: #fff;
}
</style>