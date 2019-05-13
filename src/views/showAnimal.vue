<template>
    <div id="showAnimal">
        <div class="item" id="item1">
            <ul id="list11">
                <li class="send"
                    v-for="(animal, index) in animals"
                    :key="index"
                    @click="selectAnimal(animal)"
                >{{animal.name}}</li>
            </ul>
            <ul id="list12"></ul>


        </div>
        <div class="item" id="item2">
            <ul id="list21">
                <li class="send"
                    v-for="(a, index) in animals"
                    @click="selectAnimal(a)"
                    :key="index"
                >{{a.name}}</li>
            </ul>
            <ul id="list22">
            </ul>
        </div>

        <div class="item" id="item3">
            <ul id="list31">
                <li class="send"
                    v-for="(a, index) in animals"
                    @click="selectAnimal(a)"
                    :key="index"
                >{{a.name}}</li>
            </ul>
            <ul id="list32">
            </ul>
        </div>

        <router-view />

    </div>
</template>

<script>
    import axios from 'axios/index';
    export default {
        name:'showAnimal',
        data(){
            return{
                animals: null,
                scrollMove: null,
                send1: null,//传送带
                list1: null,//传送带的项目列表
                send2: null,
                list2: null,
                send3: null,
                list3: null
            }
        },
        methods:{
            async initData() {
                let animals = await axios.get('/data/animals.json');
                this.animals = animals.data;
            },
            init(){
                this.send1 = document.getElementById("item1");
                let l11 = document.getElementById("list11");
                let l12 = document.getElementById("list12");
                l12.innerHTML = l11.innerHTML;
                this.list1 = l11;
                l12.style.marginLeft = parseInt(l11.offsetWidth) + "px";//使在同一行

                this.send2 = document.getElementById("item2");
                let l21 = document.getElementById("list21");
                let l22 = document.getElementById("list22");
                l22.innerHTML = l21.innerHTML;
                this.list2 = l21;
                l22.style.marginLeft = parseInt(l21.offsetWidth) + "px";

                this.send3 = document.getElementById("item3");
                let l31 = document.getElementById("list31");
                let l32 = document.getElementById("list32");
                l32.innerHTML = l31.innerHTML;
                this.list3 = l31;
                l32.style.marginLeft = parseInt(l31.offsetWidth) + "px";
            },
            scroll(x, y){
                if(x.scrollLeft >= y.offsetWidth){ //滚动条距离顶部的值恰好等于list1的宽度时，达到滚动临界点，此时将让scrollTop=0,让list1回到初始位置，实现无缝滚动item.scrollLeft = 0;
                    x.scrollLeft = 0;
                }else {
                    x.scrollLeft++;
                }
            },
            play(){
                this.scrollMove = setInterval(this.scroll, 30, this.send1, this.list1);//数值越大，滚动速度越慢
                setInterval(this.scroll, 30, this.send2, this.list2);
                setInterval(this.scroll, 30, this.send3, this.list3);
            },
            selectAnimal(item){
                this.$router.push(`${this.$route.path + 'showDetail/' + item.id}`);
            }

        },
        async mounted() {
            await this.initData();
            this.init();
            this.play();
        },
        created() {
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
    #list12,#list22,#list32{
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
