<template>
  <div id="FormWorkExperience">
    <div class="form_information" v-if="!loading">
      <div class="form_info_title">
        <h2>工作经历</h2>
      </div>
      <div v-for="(formWorkExp,index) in formsWorkExp">
        <el-form label-position="labelPosition" label-width="200px" :status-icon="true" class="animated fadeIn" :rules="rules"
                 :model="formWorkExp" ref="formsWorkExp">
           <div class="resume_form-items">
              <el-form-item label="起始日期" prop="startTime">
                <el-date-picker type="date" placeholder="选择日期" v-model="formWorkExp.startTime"></el-date-picker>
              </el-form-item>
              <el-form-item label="结束日期" prop="endTime">
                <el-date-picker type="date" placeholder="选择日期" v-model="formWorkExp.endTime"></el-date-picker>
              </el-form-item>
              <el-form-item label="所在公司" prop="company">
                <el-input v-model="formWorkExp.company"></el-input>
              </el-form-item>
              <el-form-item label="所任职位" prop="position">
                <el-input v-model="formWorkExp.position"></el-input>
              </el-form-item>
              <el-form-item label="详细描述" prop="description">
                <el-input type="textarea" rows="7" v-model="formWorkExp.description"></el-input>
                <span>{{formWorkExp.description.length}}/200</span>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="saveOne(index,'formsWorkExp')">保存</el-button>
                <el-button type="info" @click="deleteOne(formWorkExp.id,index)">删除</el-button>
              </el-form-item>
            </div>
          <div class="needMarginBorder"></div>
        </el-form>
      </div>
      <el-form label-position="labelPosition" label-width="200px" >
        <el-form-item label="还有其他工作经历？">
          <el-button @click="addOne()"><i class="el-icon-plus"></i></el-button>
        </el-form-item>
        <div class="needMarginBorder"></div>
        <el-form-item>
          <el-button type="primary" class="button4forms" @click="nextStep('formsWorkExp')">保存并进行下一步</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
  import { isName } from '../../util/Validate'

  var checkCompanyName = (rule, value, callback) => {
    if (!isName(value)) {
      callback(new Error('请输入正确的公司名'))
    } else {
      callback()
    }
  }
  export default {
    data () {
      var checkEndTime = (rule, value, callback) => {
        for (let index = 0; index < this.$data.formsWorkExp.length; index++) {
          if (this.$data.formsWorkExp[index].endTime === value) {
            if (this.$data.formsWorkExp[index].startTime > value) {
              callback(new Error('起始日期不能大于结束日期'))
            }
          }
        }
        callback()
      }
      return {
        loading: true,
        // formsWorkExp: null,
        formsWorkExp: [{
          id: null,
          resumeId: '',
          startTime: null,
          endTime: null,
          company: '',
          position: '',
          description: ''
        }],
        rules: {
          startTime: [
            {type: 'date', message: '请选择正确的日期', trigger: 'blur'}
          ],
          endTime: [
            {type: 'date', message: '请选择正确日期', trigger: 'blur'},
            {validator: checkEndTime, trigger: 'blur'}
          ],
          company: [
            {validator: checkCompanyName, trigger: 'change'},
            {max: 30, message: '长度超过限制', trigger: 'change'},
            {required: true, message: '请输入公司名', trigger: 'change'}
          ],
          position: [
            {validator: checkCompanyName, trigger: 'change'},
            {max: 30, message: '长度超过限制', trigger: 'change'},
            {required: true, message: '请输入职位名', trigger: 'change'}
          ],
          description: [
            {max: 200, message: '长度超过限制', trigger: 'change'},
            {required: true, message: '请输入工作内容', trigger: 'change'}
          ]
        }
      }
    },
    created () {
      let _this = this
      this.$axios({
        method: 'get',
        url: '/work'
      }).then(function (response) {
        _this.$nextTick(() => {
          if (response.data.length !== 0) {
            _this.$data.formsWorkExp = response.data
          }
          _this.$data.loading = false
        })
      })
    },
    methods: {
      nextStep (formName) {

        let flag = true
        if (this.$data.formsWorkExp.length === 0) {
          this.$message({
            message: '此页面你没有保存任何东西就进入下一步了哦',
            type: 'success'
          })
          this.$router.push('/ResumeForm/6')
        } else {
          for (let index = 0; index < this.$refs[formName].length; index++) {
            if (this.$data.formsWorkExp[index].startTime !== null) {
              this.$data.formsWorkExp[index].startTime = new Date(this.$data.formsWorkExp[index].startTime)
              this.$data.formsWorkExp[index].startTime.setTime(this.$data.formsWorkExp[index].startTime.getTime() + 8 * 3600 * 1000)
            }
            if (this.$data.formsWorkExp[index].endTime !== null) {
              this.$data.formsWorkExp[index].endTime = new Date(this.$data.formsWorkExp[index].endTime)
              this.$data.formsWorkExp[index].endTime.setTime(this.$data.formsWorkExp[index].endTime.getTime() + 8 * 3600 * 1000)
            }
            this.$refs[formName][index].validate((valid) => {
              if (!valid) {
                flag = false
              } else {
                let _this = this
                this.$axios({
                  method: 'post',
                  url: '/work',
                  data: this.$data.formsWorkExp[index]
                }).then(function (response) {
                  _this.$message({
                    message: '成功保存，进入下一步填写',
                    type: 'success'
                  })
                  _this.$data.formsWorkExp.splice(index, 1, response.data)
                })
              }
            })
          }
          if (flag === true) {
            this.$router.push('/ResumeForm/6')
          }
        }
      },
      addOne () {
        console.log('formsWorkExp', this.$data.formsWorkExp)
        this.$data.formsWorkExp.push(
          {
            id: null,
            resumeId: '',
            startTime: null,
            endTime: null,
            company: '',
            position: '',
            description: ''
          }
        )
      },
      deleteOne (num, index) {
        let _this = this
        console.log(this.$data.formsWorkExp)
        if (num === null) {
          this.$data.formsWorkExp.splice(index, 1)
          return
        }
        this.$axios({
          method: 'delete',
          url: '/work/' + num
        }).then(function (response) {
          _this.$data.formsWorkExp.splice(index, 1)
        })
      },
      saveOne (index, formName) {
        let flag = true
        if (this.$data.formsWorkExp[index].startTime !== null) {
          this.$data.formsWorkExp[index].startTime = new Date(this.$data.formsWorkExp[index].startTime)
          this.$data.formsWorkExp[index].startTime.setTime(this.$data.formsWorkExp[index].startTime.getTime() + 8 * 3600 * 1000)
        }
        if (this.$data.formsWorkExp[index].endTime !== null) {
          this.$data.formsWorkExp[index].endTime = new Date(this.$data.formsWorkExp[index].endTime)
          this.$data.formsWorkExp[index].endTime.setTime(this.$data.formsWorkExp[index].endTime.getTime() + 8 * 3600 * 1000)
        }
        this.$refs[formName][index].validate((valid) => {
          if (!valid) {
            flag = false
          }
        })
        if (flag === true) {
          let _this = this
          this.$axios({
            method: 'post',
            url: '/work',
            data: this.$data.formsWorkExp[index]
          }).then(function (response) {
            _this.$message({
              message: '此次经历保存成功',
              type: 'success'
            })
            _this.$data.formsWorkExp.splice(index, 1, response.data)
          })
        }
      }
    }
  }
</script>
<style scoped>
</style>
