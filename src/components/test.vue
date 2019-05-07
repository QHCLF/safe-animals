<template>
    <div id="test">
        <div id="wrap">
            <ul id="list">
                <li>1</li>
                <li>2</li>
                <li>3</li>
                <li>4</li>
                <li>5</li>

                <li>1</li>
                <li>2</li>
                <li>3</li>
                <li>4</li>
                <li>5</li>
            </ul>
        </div>

        <div id="box">
            <ul id="list1">
                <li>星期一</li>
                <li>星期二</li>
                <li>星期三</li>
                <li>星期四</li>
                <li>星期五</li>
                <li>星期六</li>
                <li>星期天</li>
            </ul>
            <ul id="list2"></ul>
        </div>

        <div id="test3">
            <ul id="testt1" :style="{ width: this.width + 'px' }">
                <li class="send"
                    v-for="(animal, index) in animals"
                    :key="index"
                    v-on:mouseover="stop()"
                    v-on:mouseout="run()"
                >{{animal}}</li>
            </ul>
            <ul id="testt2">
            </ul>
        </div>

    </div>
</template>

<script>
    export default {
        name:"test",
        data() {
            return {
                scrollMove: null,
                animals: ["1", "2", "3", "4"],
                width: 200
            }
        },
        mounted(){
            this.play();
        },
        methods:{
            scrollup(){
                var box=document.getElementById("box");
                var l1=document.getElementById("list1");
                var l2=document.getElementById("list2");
                l2.innerHTML = l1.innerHTML;//克隆list1的数据，使得list2和list1的数据一样
                if(box.scrollLeft >= (l1.offsetWidth)){ //滚动条距离顶部的值恰好等于list1的高度时，达到滚动临界点，此时将让scrollTop=0,让list1回到初始位置，实现无缝滚动
                    box.scrollLeft = 0;
                }else{
                    box.scrollLeft++;
                }

            },
            play(){
                this.scrollMove = setInterval(this.scrollup, 30);//数值越大，滚动速度越慢
            },
            run(){
                this.scrollMove=setInterval(this.scrollup,30);
            },
            stop(){
                clearInterval(this.scrollMove)
            }
        },
        computed:{
            init(){
                let li = document.getElementsByTagName("li")[0];
                let sum = this.animals.length * li.offsetWidth ;
                return sum;
            }
        }
    }
</script>

<style>

    #test{
        width: 100%;
        height: 100%;
        margin: 0;
        padding: 0;
    }

    ul{
        list-style: none;
        margin: 0;
        float: left;
        padding: 0;
        white-space: nowrap;
    }


    li{
        width: 65px;
        background-color: pink;
        margin-left: 2.5px;
        margin-right: 2.5px;
        white-space: normal;
        display: inline-block;
        vertical-align: middle;
    }
    #box{ width: 100%; height:90px; overflow:hidden; border:1px solid red; }
    #list2{
        margin-left: 100%;
        margin-top: -24px;
    }
    #wrap{
        /*width: 500px;*/
        width: 100%;
        height: 100px;
        border: 1px solid #000;
        position: relative;
        margin: 100px auto;
        overflow: hidden;
    }

    /*#list1 {*/
    /*    width: 50%;*/
    /*}*/
    /*#list2 {*/
    /*    width: 50%;*/
    /*}*/

    @keyframes move {
        0% {
            left: 0;
        }
        100% {
            left: -500px;
        }
    }

    #wrap:hover #list {
        -webkit-animation-play-state: paused; /*动画暂停播放*/
    }


    #test3{
        width: 100%;
        height: 120px;
        background-color: #1CB5E0;
    }
</style>
