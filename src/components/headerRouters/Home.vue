<template>
<div id="Home">
  <div id="canvas-frame" style="margin-top:-80px"></div>
  <div style="width: 100%;height: 100%;text-align: center;">
    <div style="width: 26%;min-width:300px;display: inline-block;top:40%;left:37%;position: absolute">
      <el-form style="text-align: left;">
        <el-form-item style="margin-bottom:10px">
          <div style="display: inline-block;width:100%;padding: 4px;border: solid 0.3px #ffffff;border-radius: 6px">
            <el-input :placeholder="resumeChosen==0?'你的搜索将基于 -  社会招聘  -':resumeChosen==1?'你的搜索将基于 -  校园招聘  -':'你的搜索将基于 -  实习生招聘  -'" v-model="input3" style="font-size: 18px;height: 50px;width:100%">
              <el-button slot="append" @click="searchPositions"><i class="icon iconfont icon-sousuo" style="font-size: 30px;color: transpartent"></i> </el-button>
            </el-input>
          </div>
        </el-form-item>
        <div style="margin-left:4.5%">
          <el-form-item style="margin-bottom:10px">
            <h3 style="display: inline;;margin-right: 3%;font-weight:normal;color: #ffffff;font-size: 18px">工作地点：</h3>
            <el-button type="text" style="color: #ffffff;font-weight:normal;font-size: 18px" v-for="item in places" @click="choosePlace(item.text)" :key="item.value">{{item.text}}</el-button>
          </el-form-item>
        </div>
        <div style="margin-left:4.5%">
          <el-form-item style="margin-bottom:10px">
            <h3 style="display: inline;margin-right: 3%;font-weight:normal;color: #ffffff;font-size: 18px">职位类型：</h3>
            <el-button type="text" style="color: #ffffff;font-weight:normal;font-size: 18px" v-for="item in classes" @click="chooseType(item.text)" :key="item.value">{{item.text}}</el-button>
          </el-form-item>
        </div>
      </el-form>
    </div>
  </div>
</div>
</template>

<script>
import ElFormItem from '../../../node_modules/element-ui/packages/form/src/form-item.vue'
import * as THREE from 'three'
export default {
  components: {
    ElFormItem
  },
  name: 'Home',
  data() {
    return {
      input3: '',
      value3: '',
      postChosen: '',
      placeChosen: '',
      resumeChosen: 0,
      scene: null,
      camera: null,
      renderer: null,
      material: null,
      texture: null,
      documentBodyClientWidth: 0,
      items: [{
          text: '展示示例一'
        },
        {
          text: '展示示例二'
        },
        {
          text: '展示示例三'
        },
        {
          text: '展示示例四'
        }
      ],
      places: [{
        text: '深圳'
      }, ],
      classes: [{
          text: '研发'
        },
        {
          text: '设计'
        },
        {
          text: '产品'
        },
        {
          text: '销售'
        }
      ],
      posts: [{
          text: '校园招聘'
        },
        {
          text: '社会招聘'
        },
        {
          text: '实习生招聘'
        }
      ],
      loop: null
    }
  },
  beforeRouteLeave(to, from, next) {
    next()
  },
  mounted() {
    this.init()
  },
  destroyed() {
    window.cancelAnimationFrame(this.loop)
  },
  methods: {
    init() {
      let scene = this.$data.scene
      let camera = this.$data.camera
      let renderer = this.$data.renderer
      let texture = this.$data.texture
      let material = this.$data.material
      scene = new THREE.Scene();
      camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 4, 10000);
      camera.position.z = 5;
      renderer = new THREE.WebGLRenderer({
        antialias: true,
        alpha: true,
      });
      renderer.setSize(window.innerWidth, window.innerHeight);
      renderer.setClearColor(0x1476C1, 0.2);
      document.getElementById('canvas-frame').appendChild(renderer.domElement);
      const light = new THREE.AmbientLight(0x000000); // soft white light
      scene.add(light);
      var loader = new THREE.TextureLoader()
      texture = [loader.load('../../../static/img/homeImg/she.png'),
        loader.load('../../../static/img/homeImg/xiao.png'), loader.load('../../../static/img/homeImg/shi.png')
      ]
      material = [new THREE.MeshLambertMaterial({ //贴图通过材质添加给几何体
        emissive: 0xffffff,
        transparent: true,
        alphaMap: texture[0],
        opacity: 0.9

      }), new THREE.MeshLambertMaterial({ //贴图通过材质添加给几何体
        emissive: 0xffffff,
        transparent: true,
        alphaMap: texture[1],
        opacity: 0.5
      }), new THREE.MeshLambertMaterial({ //贴图通过材质添加给几何体
        emissive: 0xffffff,
        transparent: true,
        alphaMap: texture[2],
        opacity: 0.5
      })]
      window.onresize = function temp() {
        camera.aspect = window.innerWidth / window.innerHeight;
        camera.updateProjectionMatrix();
        renderer.setSize(window.innerWidth, window.innerHeight);
      };
      var circles = []
      var cs = []
      var positions = [new THREE.Vector3(-1.5, 2, 0.5), new THREE.Vector3(-3.5, -2.5, 0), new THREE.Vector3(3.7, 0.5, 0)]
      var lines = []
      const lineMaterial = new THREE.LineDashedMaterial({
        scale: 1,
        color: 0xffffff,
        dashSize: 1,
        gapSize: 1
      });
      var pointPosition1 = []
      var pointPosition2 = []
      var geometry = []
      var TorusGeometry = new THREE.TorusGeometry(0.55, 0.01, 100, 100, Math.PI * 2)
      var CircleGeometry = new THREE.CircleGeometry(0.5, 100, 0, Math.PI * 2)
      for (var i = 0; i <= 2; i++) {
        cs[i] = new THREE.Mesh(TorusGeometry, material[i]);
        circles[i] = new THREE.Mesh(CircleGeometry, material[i])
        cs[i].position.set(positions[i].x, positions[i].y, positions[i].z)
        circles[i].position.set(positions[i].x, positions[i].y, positions[i].z)
        geometry[i] = new THREE.Geometry();
        pointPosition1[i] = new THREE.Vector3(
          positions[i].x - 0.55 / Math.sqrt(Math.pow((positions[i].x - positions[(i + 1) % 3].x), 2) + Math.pow((positions[i].y - positions[(i + 1) % 3].y), 2)) * (positions[i].x - positions[(i + 1) % 3].x),
          positions[i].y - 0.55 / Math.sqrt(Math.pow((positions[i].x - positions[(i + 1) % 3].x), 2) + Math.pow((positions[i].y - positions[(i + 1) % 3].y), 2)) * (positions[i].y - positions[(i + 1) % 3].y),
          positions[i].z
        )
        pointPosition2[i] = new THREE.Vector3(
          positions[(i + 1) % 3].x + 0.55 / Math.sqrt(Math.pow((positions[i].x - positions[(i + 1) % 3].x), 2) + Math.pow((positions[i].y - positions[(i + 1) % 3].y), 2)) * (positions[i].x - positions[(i + 1) % 3].x),
          positions[(i + 1) % 3].y + 0.55 / Math.sqrt(Math.pow((positions[i].x - positions[(i + 1) % 3].x), 2) + Math.pow((positions[i].y - positions[(i + 1) % 3].y), 2)) * (positions[i].y - positions[(i + 1) % 3].y),
          positions[(i + 1) % 3].z
        )
        geometry[i].vertices.push(pointPosition1[i]);
        geometry[i].vertices.push(pointPosition2[i]);
        geometry[i].verticesNeedUpdate = true
        lines[i] = new THREE.Line(geometry[i], lineMaterial);

      }
      circles.forEach(circle => {
        scene.add(circle)
      })
      cs.forEach(c => {
        scene.add(c)
      })
      lines.forEach(line => {
        scene.add(line)
      })
      var positionsNext = positions
      const speed = 0.00015
      var change = 0
      var angle
      var wait2up = 0
      var wait2turn = 0
      let _this = this

      var render = function() {
        if (change === 0)
          angle = [Math.random() * 2 * Math.PI, Math.random() * 2 * Math.PI, Math.random() * 2 * Math.PI]
          change = (change + 1) % 200 //
        for (i = 0; i <= 2; i++) {
          positionsNext[i] = positionsNext[i].add(new THREE.Vector3(speed * Math.sin(angle[i]), speed * Math.cos(angle[i]),
            wait2up === i ? (positionsNext[i].z >= 0.5 ? 0 : 0.01) : (positionsNext[i].z > 0 ? -0.01 : 0)
          ))
        }
        for (i = 0; i <= 2; i++) {
          const last = material[i].opacity
          material[i].opacity = last + (wait2turn === i ? (last < 0.9 ? 0.01 : 0) : (last <= 0.5 ? 0 : -0.01))
        }
        for (i = 0; i <= 2; i++) {
          circles[i].position.set(positionsNext[i].x, positionsNext[i].y, positionsNext[i].z)
          cs[i].position.set(positionsNext[i].x, positionsNext[i].y, positionsNext[i].z)
          geometry[i].vertices[0].set(
            positions[i].x - 0.55 / Math.sqrt(Math.pow((positions[i].x - positions[(i + 1) % 3].x), 2) + Math.pow((positions[i].y - positions[(i + 1) % 3].y), 2)) * (positions[i].x - positions[(i + 1) % 3].x),
            positions[i].y - 0.55 / Math.sqrt(Math.pow((positions[i].x - positions[(i + 1) % 3].x), 2) + Math.pow((positions[i].y - positions[(i + 1) % 3].y), 2)) * (positions[i].y - positions[(i + 1) % 3].y),
            positions[i].z
          )
          geometry[i].vertices[1].set(
            positions[(i + 1) % 3].x + 0.55 / Math.sqrt(Math.pow((positions[i].x - positions[(i + 1) % 3].x), 2) + Math.pow((positions[i].y - positions[(i + 1) % 3].y), 2)) * (positions[i].x - positions[(i + 1) % 3].x),
            positions[(i + 1) % 3].y + 0.55 / Math.sqrt(Math.pow((positions[i].x - positions[(i + 1) % 3].x), 2) + Math.pow((positions[i].y - positions[(i + 1) % 3].y), 2)) * (positions[i].y - positions[(i + 1) % 3].y),
            positions[(i + 1) % 3].z
          )
          geometry[i].verticesNeedUpdate = true;
        }
        renderer.render(scene, camera);
        _this.loop = requestAnimationFrame(render);
        console.log('loop',_this.loop)
      };
      var raycaster = new THREE.Raycaster();
      var mouse = new THREE.Vector2();

      function onMouseClick(event) {
        //通过鼠标点击的位置计算出raycaster所需要的点的位置，以屏幕中心为原点，值的范围为-1到1.
        mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
        mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
        // 通过鼠标点的位置和当前相机的矩阵计算出raycaster
        raycaster.setFromCamera(mouse, camera);
        // 获取raycaster直线和所有模型相交的数组集合
        var intersects = raycaster.intersectObjects(scene.children);
        //将所有的相交的模型的颜色设置为红色，如果只需要将第一个触发事件，那就数组的第一个模型改变颜色即可
        intersects.forEach((intersect) => {
          if (intersect.object.type === 'Mesh') {
            circles.forEach((circle, index) => {
              if (intersect.object === circle) {
                wait2turn = index
                wait2up = index
                _this.$data.resumeChosen = wait2turn
              }
            })
          }
        })
      }
      function onMouseHover(event) {
        //通过鼠标点击的位置计算出raycaster所需要的点的位置，以屏幕中心为原点，值的范围为-1到1.
        mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
        mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;
        // 通过鼠标点的位置和当前相机的矩阵计算出raycaster
        raycaster.setFromCamera(mouse, camera);
        // 获取raycaster直线和所有模型相交的数组集合
        var intersects = raycaster.intersectObjects(scene.children);
        wait2turn = wait2up
        //将所有的相交的模型的颜色设置为红色，如果只需要将第一个触发事件，那就数组的第一个模型改变颜色即可
        intersects.forEach((intersect) => {
          if (intersect.object.type === 'Mesh') {
            circles.forEach((circle, index) => {
              if (intersect.object === circle) {
                wait2turn = index
              }
            })
          }
        })
      }
      window.addEventListener('click', onMouseClick, false);
      window.addEventListener('mousemove', onMouseHover, false);
      render();
    },
    handleSelect(key, keyPath) {
      console.log(key, keyPath)
    },
    choosePlace(item) {
      this.$data.placeChosen = item;
      this.searchPositions()
    },
    chooseType(item) {
      this.$data.postChosen = item;
      this.searchPositions()
    },
    searchPositions() {
      let typeChosen = this.$data.postChosen;
      let positionName = this.$data.input3;
      let placeChosen = this.$data.placeChosen;
      this.$data.postChosen = '';
      this.$data.input3 = '';
      this.$data.placeChosen = '';
      console.log(this.$data.resumeChosen);
      if (this.$data.resumeChosen === 1) {
        this.$router.push({
          path: '/Campus/Post',
          query: {
            typeChosen: typeChosen,
            positionName: positionName,
            placeChosen: placeChosen
          }
        })
      }
      if (this.$data.resumeChosen === 0) {
        this.$router.push({
          path: '/Social',
          query: {
            typeChosen: typeChosen,
            positionName: positionName,
            placeChosen: placeChosen
          }
        })
      }
      if (this.$data.resumeChosen === 2) {
        this.$router.push({
          path: '/Trainee',
          query: {
            typeChosen: typeChosen,
            positionName: positionName,
            placeChosen: placeChosen
          }
        })
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less">
  #Home {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: left;
    color: #2c3e50;
    margin-top: -20px;
    font-size: 18px;
    #canvas-frame { width: 100%; height: 100%;background:url('../../../static/img/main1.jpg') no-repeat;background-size:100% 100%}
    .el-input__inner{
      border:2px solid #fafafa;

      height: 50px;
      color:#ffffff;
      background: transparent !important;
    }

    .el-input-group__append{
      border:1px solid #fafafa;
      background: #ffffff;
    }
  }
</style>
