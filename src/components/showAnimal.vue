<template>
    <div id="showAnimal">
        <div class="item" id="item">
            <ul id="list11">
                <li class="send"
                    v-for="(animal, index) in animals"
                    :key="index"
                    v-on:mouseover="stop()"
                    v-on:mouseout="run()"
                >{{animal}}</li>
            </ul>
            <ul id="list12"></ul>
        </div>
    </div>
</template>

<script>
    export default {
        name:'showAnimal',
        data(){
            return{
                animals: ["1", "2", "3", "4", "5"],
                scrollMove: null
            }
        },
        methods:{
            scroll(){
                let item = document.getElementById("item");
                let l1 = document.getElementById("list11");
                let l2 = document.getElementById("list12");
                l2.innerHTML = l1.innerHTML;
                l2.style.marginLeft = parseInt(l1.offsetWidth) + "px";//使在同一行
                if(item.scrollLeft >= l1.offsetWidth){ //滚动条距离顶部的值恰好等于list1的宽度时，达到滚动临界点，此时将让scrollTop=0,让list1回到初始位置，实现无缝滚动item.scrollLeft = 0;
                    item.scrollLeft = 0;
                }else {
                    item.scrollLeft++;
                }

            },
            play(){
                this.scrollMove = setInterval(this.scroll, 30);//数值越大，滚动速度越慢
            },
            run(){
                this.scrollMove=setInterval(this.scroll,30);
            },
            stop(){
                clearInterval(this.scrollMove)
            }

        },
        mounted() {
            this.play();
        }
    }

</script>

<style lang="stylus" scoped>
    #showAnimal{
        width: 100%;
        height: 100%;
        margin: 0;
    }


    .item{
        width: 100%;
        height: 110px;
        background-color: #42b983;
        margin 0;
        margin-bottom: 12px;
        overflow: hidden;
    }
    ul{
        list-style: none;
        margin: 0;
        float: left;
        padding: 0;
        white-space: nowrap;
    }
    #list12{
        margin-top -110px;
    }
    .send{
        width: 110px;
        height 110px;
        background-color: pink;
        white-space: normal;
        display: inline-block;
        vertical-align: middle;
    }




</style>
