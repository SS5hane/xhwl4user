<template>
  <div id="PostCard" style="z-index: 999;min-height: 650px">
    <div v-if="cardInfo.length===0" style="width:100%;">
    <h1 style="color:#ffffff;margin-top: 100px;position: absolute;z-index: 1000">没有您所要检索的职位、请重新筛选！</h1>
    </div>
    <el-card v-else v-for="item in cardInfo" class="box-card animated fadeIn" v-bind:key="item.index"
             v-bind:style="'z-index: 999;margin-top:2%;width:100%;background-color:#ffffff;animation-delay:'+(item.index)*0.2+'s'">
      <div>
        <span style="font-size:24px;font-weight: bold" class="name"> {{item.positionName}}</span>
        <span style="font-size:18px;" class="name">  {{item.positionType}}</span>
        <span style="float:right;font-size:14px;margin-top: 10px">发布于 {{item.publishDate}}</span>
      </div>

      <div style="height:20px;" class="blank"></div>

      <div class="brief" style="font-weight:normal;font-size:16px;">
        <div style="margin-top: 1%">
          <a style="font-size: 20px"> <i class="icon iconfont icon-gongzuodidian-shixin icon4card"></i>     工作地点：{{item.workPlace}}</a>
        </div>
        <div style="margin-top: 1%;">
          <i class="icon iconfont icon-suozaibumen- icon4card"></i>
          <a  style="font-size: 20px">所属部门：{{item.department}}</a>
        </div>
      </div>

      <div style="height:20px;" class="blank"></div>
      <div id="details" style="font-weight:normal;font-size:15px;">

        <el-collapse-transition>
          <div v-show="item.Ashow" >
            <div style="margin-top: 1%">
              <div style="display:block">
                <i class="icon iconfont icon-chakanxiangqing icon4card"></i>
                <a style="font-size: 20px">岗位描述：</a>
              </div>
              <div style="display: block;margin-top: 1%;margin-left:120px">
                <a style="white-space:pre-wrap;font-size: 20px">{{item.jobResponsibilities}}</a>
              </div>
            </div>
            <div style="margin-top: 1%">
              <div style="display:block">
                <i class="icon iconfont icon-gongzuoyaoqiu icon4card"></i>
                <a style="font-size: 20px">岗位要求：</a>
              </div>
              <div style="display: block;margin-top: 1%;margin-left:120px">
                <a style="white-space:pre-wrap;font-size: 20px">{{item.jobRequirements}}</a>
              </div>
            </div>
          </div>

        </el-collapse-transition>
      </div>
      <div style="position: relative;margin-bottom:2%;margin-top: 2%;float: right" >
        <el-button type="text"   @click="item.Ashow=!item.Ashow;" style="font-size: 20px"
        >{{item.Ashow?'收起详情':'查看详情'}}</el-button>
        <el-button class="button-red" @click="send(item.index)" >立即申请</el-button>

      </div>
    </el-card>
  </div>
</template>

<script>
  import ElCard from 'element-ui/packages/card/src/main'
  import ElButtonGroup from '../../../node_modules/element-ui/packages/button/src/button-group.vue'

  export default {
    components: {
      ElButtonGroup,
      ElCard},
    props: {
      resumeForm: '',
      classChosen:'',
      postChosen:'',
      placeChosen:''
    },
    mounted () {
      let _this = this
      this.$axios({
        method: 'get',
        url: '/positions/' + this.$props.resumeForm+'?positionType='+this.$props.classChosen+
              '&workPlace='+this.$props.placeChosen+'&positionName='+this.$props.postChosen
      }).then(function (response) {
        _this.$nextTick(() => {
          response.data.forEach((item, index) => {
            item['index'] = index
            item['Ashow'] = false
            item['department'] = _this.$data.typeTable[item['department']-1]
            _this.$data.cardInfo.push(item)
          })
          console.log(_this.$data.cardInfo)
        })
      })
      this.$data.kind=this.$props.resumeForm==='1'?'校招':this.$props.resumeForm==='2'?'社招':'实习生'
    },
    data () {
      return {
        kind: '',
        cardInfo: [],
        typeTable:['人事行政部', '财务管理部', '部门管理部', '市场开发部', '工程技术部',
          '运维及质量安全部', '研发设计部', '华南办事处', '深圳办事处', '北方办事处',
          '西部办事处', '华东办事处', '华中办事处', '华北办事处']
      }
    },
    watch:{
      //监测到selector变化时的函数
      placeChosen(val,oldval){
        console.log(this.$props.placeChosen);
        let _this = this
        this.$axios({
          method: 'get',
          url: '/positions/' + this.$props.resumeForm+'?positionType='+this.$props.classChosen+
          '&workPlace='+this.$props.placeChosen+'&positionName='+this.$props.postChosen
        }).then(function (response) {
          _this.$nextTick(() => {
            _this.$data.cardInfo.splice(0,_this.$data.cardInfo.length);
            response.data.forEach((item, index) => {
              item['index'] = index
              item['Ashow'] = false
              item['department'] = _this.$data.typeTable[item['department']-1]
              _this.$data.cardInfo.push(item)
            })
            console.log(_this.$data.cardInfo)
          })
        })

      },
      postChosen(val,oldval){
        let _this = this
        this.$axios({
          method: 'get',
          url: '/positions/' + this.$props.resumeForm+'?positionType='+this.$props.classChosen+
          '&workPlace='+this.$props.placeChosen+'&positionName='+this.$props.postChosen
        }).then(function (response) {
          _this.$nextTick(() => {
            _this.$data.cardInfo.splice(0,_this.$data.cardInfo.length);
            response.data.forEach((item, index) => {
              item['index'] = index
              item['Ashow'] = false
              item['department'] = _this.$data.typeTable[item['department']-1]
              _this.$data.cardInfo.push(item)
            })
            console.log(_this.$data.cardInfo)
          })
        })

      },
      classChosen(val,oldval){
        let _this = this
        this.$axios({
          method: 'get',
          url: '/positions/' + this.$props.resumeForm+'?positionType='+this.$props.classChosen+
          '&workPlace='+this.$props.placeChosen+'&positionName='+this.$props.postChosen
        }).then(function (response) {
          _this.$data.cardInfo.splice(0,_this.$data.cardInfo.length);
          _this.$nextTick(() => {
            response.data.forEach((item, index) => {
              item['index'] = index
              item['Ashow'] = false
              item['department'] = _this.$data.typeTable[item['department']-1]
              _this.$data.cardInfo.push(item)
            })
            console.log(_this.$data.cardInfo)
          })
        })
      },
    },
    methods: {
      send (index) {
        let _this=this
        if (this.$axios.defaults.headers.Authorization == null) {
          _this.$message({
            type: 'warning',
            message: '请先登录并填写简历才能投递哦!'
          })
          return
        }
        this.$confirm('确定投递职位' + _this.$data.cardInfo[index].positionName + '吗?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          _this.$axios({
            method: 'put',
            url: '/deliver/' + _this.$data.cardInfo[index].id
          }).then(function (response) {
            _this.$message({
              type: 'success',
              message: '提交投递成功!'
            })
          }).catch(function (error) {
            _this.$message({
              type: 'error',
              message: error.response.data.msg
            })

          })
        })
      }
    }
  }
</script>

<style lang="less">
  #PostCard {
    .button-red{
      border-radius: 16px;
      background: #E01B2F !important;
      border: 1px solid #E01B2F;
      /* 保存并进行下一步: */
      color: #FFFFFF !important;
      font-size: 20px;
      transition: all 0.3s;
      padding: 10px 40px;
    }
    .button-red:hover {
      background: #E01B2F;
      border: 1px solid #E01B2F;
      /* 保存并进行下一步: */
      color: #FFFFFF;
      transform: scale(1.05);
    }
    .box-card{
      transition: all 0.6s;
    }
    .el-card__body{
        padding:30px;
      }
  }
  .transition-box {
    margin-bottom: 10px;
    width: 200px;
    height: 100px;
    border-radius: 4px;
    background-color: #409EFF;
    text-align: center;
    color: #fff;
    padding: 40px 20px;
    box-sizing: border-box;
    margin-right: 20px;
  }
  .slide-fade-enter-active {
    transition: all .3s ease;
  }
  .slide-fade-leave-active {
    transition: all .3s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }
  .slide-fade-enter, .slide-fade-leave-to
    /* .slide-fade-leave-active for below version 2.1.8 */ {
    transform: translateX(20px);
    opacity: 0;
  }

</style>
