<template>
    <div id="showDetail">
        <div class="box">
            <i class="iconfont icon-left" id="left"></i>
            <div class="left"></div>
            <div class="right"></div>
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
            <i class="iconfont icon-right" id="right"></i>
        </div>

    </div>
</template>

<script>
    export default {
        name: "showDetail",
        data(){
            return{
                posY: 135,
                allowNextFrame: true,
                isDragging: false
            }
        },
        methods:{
            updatePos() {
                let dom = document.getElementById("showDetail");
                this.posY = this.pageY - this.$el.getBoundingClientRect().left;
                if (this.posY < 125) this.posY = 125;
                if (this.posY > dom.offsetHeight) this.posY = dom.offsetHeight + 50;


                let center = document.getElementsByClassName("center")[0];
                let open  = document.getElementById("open");
                let left = document.getElementsByClassName("left")[0];
                let right = document.getElementsByClassName("right")[0];
                let box = document.getElementsByClassName("box")[0];
                if(this.posY === 450){
                    center.classList.add("disappear");
                    open.classList.add("disappear");
                    left.classList.add("openBoxLeft");
                    right.classList.add("openBoxRight");
                    box.classList.add("openBox");
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
            }
        },
        created() {
            window.addEventListener('mouseup', this.onMouseUp);
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
    }
    .box{
        width 316.26px;
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
        left: 48%;
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
            transform rotateX(25deg) rotateY(125deg) translateY(-15px);
        }
    @keyframes openBoxRight
        0%{
            transform rotateX(0) rotateY(0) translateY(0);
        }
        100%{
            transform rotateX(-25deg) rotateY(125deg) translateY(-15px);
        }
    @keyframes openBox
        0%{
            width 316.26px;
            height 300px;
        }
        100%{
            width 225px;
            height 275px;
        }





</style>
