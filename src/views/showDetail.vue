<template>
    <div id="showDetail">
        <i class="iconfont icon-left" id="left" @click="prev"></i>
        <div class="box">
            <div class="left"></div>
            <div class="right"></div>
            <div class="contain">
            </div>
            <div class="center">
                <div class="slid"></div>
            </div>
            <div class="open" id="open"
                 @mousemove.prevent = "onMouseMove"
                 @touchstart = "onMouseMove($event, true)"
                 @touchmove = "onMouseMove($event, true)"
                 @mousedown.prevent="onMouseDown"
                 :style="{ top: posY + 'px' }"
            >
            </div>
        </div>
        <i class="iconfont icon-right" id="right" @click="next"></i>

    </div>
</template>

<script>
    import axios from 'axios/index';
    export default {
        name: "showDetail",
        data(){
            return{
                posY: 125,
                allowNextFrame: true,
                isDragging: false,
                id: this.$route.params.detailId,
                AnimalList: null,
                currentAnimal: null
            }
        },
        methods:{
            updatePos() {
                let dom = document.getElementById("showDetail");
                this.posY = this.pageY - this.$el.getBoundingClientRect().left;
                if (this.posY < 125) this.posY = 125;
                console.log(dom.offsetHeight);
                if (this.posY > dom.offsetHeight) this.posY = dom.offsetHeight - 25;


                let center = document.getElementsByClassName("center")[0];
                let open  = document.getElementById("open");
                let left = document.getElementsByClassName("left")[0];
                let right = document.getElementsByClassName("right")[0];
                let box = document.getElementsByClassName("box")[0];
                let contain = document.getElementsByClassName("contain")[0];
                if(this.posY === 450){
                    center.classList.add("disappear");
                    open.classList.add("disappear");
                    left.classList.add("openBoxLeft");
                    right.classList.add("openBoxRight");
                    box.classList.add("openBox");
                    contain.innerHTML = this.currentAnimal.name;
                }

                this.allowNextFrame = true;
            },
            onMouseDown() {
                this.isDragging = true;
            },

            onMouseUp(event) {
                event.preventDefault();
                this.isDragging = false;
            },
            onMouseMove(event, isDragging = this.isDragging) {
                if (isDragging && this.allowNextFrame) {
                    this.allowNextFrame = false;
                    this.pageY = event.pageY || event.targetTouches[0].pageY || event.originalEvent.targetTouches[0].pageY;
                    window.requestAnimationFrame(this.updatePos);
                }
            },
            async getAnimalList(){
                let animals = await axios.get('/data/animals.json');
                this.AnimalList = animals.data;
            },
            prev(){
                if(this.AnimalList.length > 0 && this.AnimalList.length !== 1){
                    if(this.id === 0)
                        this.id = 0;
                    else
                        this.id--;
                }
            },
            next(){
                if(this.AnimalList.length > 0 && this.AnimalList.length !== 1){
                    if(this.id === this.AnimalList.length - 1)
                        this.id = this.AnimalList.length - 1;
                    else
                        this.id++;
                }
            }



        },
        computed :{
            getCurrent(){
                this.currentAnimal = this.AnimalList[this.id];
                return this.currentAnimal;
            }
        },
        created() {
            window.addEventListener('mouseup', this.onMouseUp);
        },
        mounted(){
            this.getAnimalList();
        },
        beforeDestroy() {
            window.removeEventListener('mouseup', this.onMouseUp);
        }
    }
</script>

<style lang="stylus" scoped>
    @import '//at.alicdn.com/t/font_1180345_6lvk36vsqlx.css';
    #showDetail{
        width 100%
        height 400px;
        margin: 0;
        padding: 0;
        text-align: center;
        padding-top 75px;
    }
    .box{
        width 285px;
        height: 300px;
        background-color: pink;
        margin: 0 auto;
        margin-top: 80px;

    }
    .box.openBox{
        animation openBox 1.5s linear;
        animation-fill-mode forwards;
    }
    .left, .right{
        width 50%;
        height 300px;
        background-color #42b983;
        float left;
    }
    .left.openBoxLeft{
        transform-origin left;
        animation openBoxLeft 1.5s linear;
        animation-fill-mode forwards;
    }
    .right.openBoxRight{
        transform-origin right;
        animation openBoxRight 1.5s linear;
        animation-fill-mode forwards;
    }
    .contain{
        position relative;
        top: -175px;
    }
    #left{
        position: absolute;
        left: 2%;
        top: 45%;
        font-size: 36px;
    }
    #right{
        position: absolute;
        right 2%;
        top: 45%;
        font-size 36px;
    }
    .center{
        margin: auto;
        width: 20%;
        height: 100%;
        background-color: #a52a2a;
        position: relative;
    }

    .slid{
        margin: auto;
        width: 2%;
        height: 100%;
        background-color: #42b983;
    }

    .open{
        margin: auto;
        width: 8%;
        height: 10%;
        background-color: #a52a2a;
        position: absolute;
        top: 17%;
        left: 50%;
        transform: rotate(-30deg);
        border 1px solid gray;
    }

    .disappear{
        transition: All 0.4s ease-in-out;
        display none;
    }

    @keyframes openBoxLeft
        0%{
            transform rotateX(0) rotateY(0) translateY(0);
        }
        100%{
            transform rotateX(25deg) rotateY(120deg) translateY(-15px);
        }
    @keyframes openBoxRight
        0%{
            transform rotateX(0) rotateY(0) translateY(0);
        }
        100%{
            transform rotateX(-25deg) rotateY(120deg) translateY(-15px);
        }
    @keyframes openBox
        0%{
            width 285px;
            height 300px;
        }
        100%{
            width 225px;
            height 275px;
        }





</style>
