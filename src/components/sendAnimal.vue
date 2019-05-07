<template>
    <div id="sendAnimal">
        <div id="bird"></div>
    </div>
</template>

<script>
    import * as THREE from 'three'
    import {MTLLoader, OBJLoader} from 'three-obj-mtl-loader'
    import OrbitControls from 'three-orbitcontrols'

    export default {
        name:"sendAnimal",
        data(){
            return{
                camera: null,
                renderer: null,
                scene: null,
                mesh: null
            }
        },
        methods:{
            init(){
                let bird = document.getElementById('bird');
                let scene = new THREE.Scene();
                let mtlloader = new MTLLoader();
                let objloader = new OBJLoader();


                this.camera = new THREE.PerspectiveCamera( 45, window.innerWidth / window.innerHeight, 0.01, 500 );
                this.camera.lookAt( scene.position );
                this.camera.position.set(0.000002039256185537032, 90, -2.033825520564871);
                this.camera.scale.z = 0.1;

                let ambientLight = new THREE.AmbientLight( 0xcccccc, 0.4 );
                scene.add( ambientLight );

                let pointLight = new THREE.PointLight( 0xffffff, 0.8 );
                this.camera.add( pointLight );
                scene.add( this.camera );

                mtlloader.setPath( 'models/dolphin/')
                    .load( '10014_dolphin_v2_max2011_it2.mtl', function ( materials ) {
                        materials.preload();
                        objloader.setMaterials( materials )
                            .setPath( 'models/dolphin/' )
                            .load( '10014_dolphin_v2_max2011_it2.obj', function ( object ) {
                                // object.position.y = - 95;
                                scene.add( object );
                            });
                    });



                let controls = new OrbitControls(this.camera);// 初始化控制器
                controls.target.set(0, 0, 0);// 设置控制器的焦点，使控制器围绕这个焦点进行旋转
                controls.minDistance = 0.1;// 设置移动的最短距离（默认为零）
                controls.maxDistance = 400;// 设置移动的最长距离（默认为无穷）
                controls.maxPolarAngle = Math.PI / 3;//绕垂直轨道的距离（范围是0-Math.PI,默认为Math.PI）
                controls.update();// 照相机转动时，必须更新该控制器


                this.renderer = new THREE.WebGLRenderer({ antialias: true,alpha:true });
                this.renderer.setPixelRatio( window.devicePixelRatio );
                this.renderer.setSize( bird.clientWidth, bird.clientHeight );
                bird.appendChild( this.renderer.domElement );
                this.scene = scene;



            },
            animate(){
                requestAnimationFrame( this.animate );
                this.renderer.render(this.scene, this.camera);
            }
        },
        mounted() {
            this.init();
            this.animate();
        }
    }
</script>

<style>
    #sendAnimal{
        width: 100%;
        height: 100%;
        background-color: #42b983;
    }

    #bird{
        width: 200px;
        height: 200px;
    }


</style>
