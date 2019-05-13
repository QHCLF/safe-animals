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
                step: 0
            }
        },
        methods:{
            init(){
                let bird = document.getElementById('bird');
                let scene = new THREE.Scene();
                let mtlloader = new MTLLoader();
                let objloader = new OBJLoader();


                this.camera = new THREE.PerspectiveCamera( 45, window.innerWidth / window.innerHeight, 0.01, 2000 );
                this.camera.lookAt( scene.position );
                this.camera.position.set(-200, 300, 400);
                //this.camera.position.set(0.000002039256185537032, 90, -2.033825520564871);
                this.camera.scale.z = 2.4;



                let ambientLight = new THREE.AmbientLight( 0xcccccc, 0.4 );
                scene.add( ambientLight );

                let pointLight = new THREE.PointLight( 0xffffff, 0.8 );
                this.camera.add( pointLight );
                scene.add( this.camera );


                let planeGeometry=new THREE.PlaneGeometry(60,20,10,10);//注意参数
                //生成一个材质，设置材质的颜色，同时显示线框
                let planeMaterial=new THREE.MeshBasicMaterial({color:0xcccccc,wireframe:true});
                //生成一个网格，将平面和材质放在一个网格中，组合在一起，组成一个物体
                let plane=new THREE.Mesh(planeGeometry,planeMaterial);
                plane.rotation.x=-0.5*Math.PI;
                plane.position.x=15;
                plane.position.y=0;
                plane.position.z=0;
                scene.add(plane);




                mtlloader.setPath( 'models/Giraffe/')
                    .load( '10021_Giraffe_v04.mtl', function ( materials ) {
                        materials.preload();
                        objloader.setMaterials( materials )
                            .setPath( 'models/Giraffe/' )
                            .load( '10021_Giraffe_v04.obj', function ( object ) {
                                object.rotation.x=-0.5*Math.PI;
                                object.position.x=15;
                                object.position.y=0;
                                object.position.z=0;
                                scene.add( object );
                            });
                    });





                let controls = new OrbitControls(this.camera);// 初始化控制器
                controls.autoRotate = true;//设置平面自动旋转

                controls.target.set(0, 0, 0);// 设置控制器的焦点，使控制器围绕这个焦点进行旋转
                controls.minDistance = 0.1;// 设置移动的最短距离（默认为零）
                controls.maxDistance = 400;// 设置移动的最长距离（默认为无穷）
                controls.maxPolarAngle = Math.PI;//绕垂直轨道的距离（范围是0-Math.PI,默认为Math.PI）
                controls.update();// 照相机转动时，必须更新该控制器


                this.renderer = new THREE.WebGLRenderer({ antialias: true,alpha:true });
                this.renderer.setPixelRatio( window.devicePixelRatio );
                this.renderer.setSize( bird.clientWidth, bird.clientHeight );
                bird.appendChild( this.renderer.domElement );
                this.scene = scene;



            },
            animate(){
                requestAnimationFrame( this.animate );
                this.render();
            },
            render(){
                this.camera.lookAt( this.scene.position );
                this.renderer.render( this.scene, this.camera );
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
        height: 100px;
        background-color: #42b983;
    }

    #bird{
        width: 200px;
        height: 200px;
    }


</style>
