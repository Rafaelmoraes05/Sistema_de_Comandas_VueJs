<template>
    <div id="main-bunner">
        <h1>Make Your Burger</h1>
    </div>
</template>
<script>
export default {
    name:'Banner'
}
</script>

<style scoped>
    #main-bunner{
        background-image: url('/img/burger.jpg');
        background-position: 0 -250px;
        background-size: cover;
        height: 500px;
        display:flex;
        align-items: center;
        justify-content: flex-start;
    }
    #main-bunner h1{
        color: #FFF;
        text-align: center;
        font-size: 60px;
        background-color: #222;
        padding: 20px 40px;
    }
    @media(min-width: 1600px){
        #main-bunner{
            background-position: 0 -500px;
        }
    }

    /*Mobile*/
    @media (max-width: 600px) {
        #main-bunner{
            height: 300px;
            background-position: 0 0;
        }
        h1{
            font-size: 30px !important;
            padding: 15px !important;
        }
    }

    /* Tablets and small desktops */
    @media (min-width: 601px) and (max-width: 1200px) {
       #main-bunner{
        height: 300px;
       }
       h1{
        font-size: 45px !important;
        padding: 15px 30px !important;
       }
       @media(max-width:850px){
            #main-bunner{
                height: 300px;
                background-position: 0 -50px;
            }
            h1{
                font-size: 30px !important;
                padding: 15px !important;
            }
       }
    }
</style>