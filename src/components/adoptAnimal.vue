<template>
    <div id="adoptAnimal">
        <div class="contain">
            <div class="disc">

            </div>
            <div class="pic">
                <img src="../assets/1.jpeg">
            </div>
        </div>
        <button class="adopt">领养Adopt</button>
    </div>
</template>
<style>
    #adoptAnimal{
        margin: 0;
        padding: 0;
    }
    .contain{
        width: 100%;
        height: 500px;
        background-color: #42b983;
    }

    .disc{
        width: 43%;
        height: 100%;
        background-color: #1CB5E0;
        float: left;
    }

    .pic{
        width: 55%;
        height: 70%;
        background-color: brown;
        float: right;
    }
    img{
        width: 100%;
    }

    button{
        width: 30%;
        height: 65px;
        border-radius: 20px;
        margin-top: 5%;
        font-size: 19px;
        margin-left: 5%;
    }


</style>
