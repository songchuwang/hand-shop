<template>
  <div class="container">
    <div class="bread-nav">
      <span class="bread-nav-first">
        首页 <i class="el-icon-arrow-right"></i>
      </span>
      <span class="bread-nav-second">
        发布页
      </span>
    </div>
    <div class="content">

      <el-form :model="ruleForm" :rules="rules" ref="ruleForm" class="demo-ruleForm">
        <el-form-item label="商品标题" prop="gname">
          <el-input v-model="ruleForm.gname"></el-input>
        </el-form-item>
        <el-form-item label="描述下你的商品吧" prop="desc">
          <el-input type="textarea" v-model="ruleForm.desc"></el-input>
        </el-form-item>

        <el-form-item prop="imgs">
          <div>上传头像<span>多角度拍摄商品细节，全面展示商品；图片大小不能超过2M哦~</span></div>
          <el-upload 
          :on-success="handleImgSuccess" 
          :on-remove="handleRemove" 
          :on-preview="handlePictureCardPreview"
          
          accept=".jpeg, .jpg, .png" 
          :file-list="ruleForm.imgs" 
          list-type="picture-card"
          action="https://jsonplaceholder.typicode.com/posts/">
            <i class="el-icon-plus"></i>
          </el-upload>
          <el-dialog :visible.sync="dialogVisible">
            <img width="100%" :src="dialogImageUrl" alt="">
          </el-dialog>
        </el-form-item>
<!-- :on-change="toBase64" -->


        <div style="margin-bottom:10px">发布地址</div>
        <el-row>
          <el-col :span="12">
            <el-form-item prop="address">
              <el-input placeholder="请输入详细地址" v-model="ruleForm.address"></el-input>
            </el-form-item>
          </el-col>
        </el-row>


        <el-form-item label="想卖多少钱">
        </el-form-item>
        <el-row>
          <el-col :span="3" style="height:40px;line-height: 40px;">售卖价：</el-col>
          <el-col :span="9">
            <el-form-item prop="sale_price">
              <el-input v-model="ruleForm.sale_price"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="3" style="height:40px;line-height: 40px;padding-left:40px">邮费：</el-col>
          <el-col :span="9">
            <el-form-item prop="postage">
              <el-input placeholder="填0则为包邮" v-model="ruleForm.postage"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="3" style="height:40px;line-height: 40px;">原价：</el-col>
          <el-col :span="9">
            <el-form-item prop="original_price">
              <el-input v-model="ruleForm.original_price"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12" style="height:40px;line-height: 40px;">（选填）</el-col>
        </el-row>

        <el-form-item>
          <el-select v-model="ruleForm.value" placeholder="请选择">
            <el-option v-for="(item, index) in ruleForm.options" :key="index" :label="item.label" :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item style="margin-top:30px;">
          <el-row>
            <el-col :span="6">
              <el-button size="medium" type="primary" @click="submitForm('ruleForm')">确认发布</el-button>
            </el-col>
          </el-row>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
  export default {
    layout: 'default',
    data() {
      return {
        input: '',
        dialogImageUrl: '',
        user:'',
        dialogVisible: false,
        base64_img: [],
        ruleForm: {
          gname: '',
          imgs: [],
          desc: '',
          address: '',
          sale_price: '',
          postage: '',
          original_price: '',
          value: '',
          options:[{
              value: 'fiction',
              label: '小说',
              kind: 'literature'
            }, {
              value: 'inspirational',
              label: '励志',
              kind: 'literature'
            }, {
              value: 'literature',
              label: '文学',
              kind: 'literature'
            }, {
              value: 'biography',
              label: '传记',
              kind: 'literature'
            }, {
              value: 'philosophy',
              label: '哲学',
              kind: 'science'
            }, {
              value: 'political',
              label: '政治',
              kind: 'science'
            }, {
              value: 'economic',
              label: '经济',
              kind: 'management'
            }, {
              value: 'management',
              label: '管理',
              kind: 'management'
            }, {
              value: 'sexuality',
              label: '两性',
              kind: 'life'
            }, {
              value: 'health',
              label: '养生',
              kind: 'life'
            }, {
              value: 'computer',
              label: '计算机',
              kind: 'education'
            }, {
              value: 'foreign-language',
              label: '外语',
              kind: 'education'
            }, {
              value: 'fitness',
              label: '健身',
              kind: 'physical'
            }, {
              value: 'motion',
              label: '运动',
              kind: 'physical'
            }, {
              value: 'anime',
              label: '动漫',
              kind: 'literature'
            }, {
              value: 'magazine',
              label: '杂志',
              kind: 'life'
            }]

        },
        rules: {
          gname: [{
            required: true,
            message: '请输入商品标题',
            trigger: 'blur'
          }],
          desc: [{
            required: true,
            message: '请添加商品描述',
            trigger: 'blur'
          }],
          address: [{
            required: true,
            message: '请输入详细地址',
            trigger: 'blur'
          }],
          sale_price: [{
            required: true,
            message: '请填写出售价格',
            trigger: 'blur',

            validator: (rule, value, callback) => {
              if (isNaN(Number(value))) {
                this.$message.error('售卖价请输入数字')
              } else {
                callback()
              }

            }
          }],
          original_price: [{
            trigger: 'blur',
            validator: (rule, value, callback) => {
              if (isNaN(Number(value))) {
                this.$message.error('原价请输入数字')
              } else {
                callback()
              }

            }
          }],
          postage: [{
            required: true,
            message: '请填写邮费，为0则包邮',
            trigger: 'blur',
            validator: (rule, value, callback) => {
              if (isNaN(Number(value))) {
                this.$message.error('邮费请输入数字')
                return false
              } else {
                callback()
              }

            }
          }],
          region: [{
            required: true,
            message: '请选择活动区域',
            trigger: 'change'
          }],
        }
      }
    },
    methods: {
      toBase64(file, fileList) {
        console.log(file, fileList);
        // let reader = new FileReader(file);
        // reader.readAsDataURL(file.raw)
        // reader.onload = ()=> {
        //   let _base64 = reader.result
        //   let BASE64 = _base64.split(',')
        //   this.imgs = Object.assign({}, this.imgs,{
        //     name: 'base64图片',
        //     url: BASE64[1]
        //   })
        //   this.imgs = [this.imgs]
        //   console.log(this.imgs);
        // }
        
        // this.imgs.name = file.name
        
      },
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          // console.log(this.ruleForm);
          if (valid) {
            let type = this.ruleForm.options.filter((item) => {
              return item.value == this.ruleForm.value
            });
            this.$axios.post('/goods/release', {
              publisher: this.$store.state.geo.userId,
              gname: this.ruleForm.gname,
              desc: this.ruleForm.desc,
              imgs: this.base64_img,
              address: this.ruleForm.address,
              sale_price: this.ruleForm.sale_price,
              postage: this.ruleForm.postage,
              original_price: this.ruleForm.original_price,
              type: type,
              user_publisher:decodeURIComponent(this.user) 


            }).then(res => {
              if(res.status == 200){
                this.$message.success('发布成功')
              }
            })
          } else {
            this.$message.error('请输入必要信息')
          }
        });
      },
      handleImgSuccess(response, file, fileList) {
        console.log(response, file, fileList);
        
        let reader = new FileReader(file.url);
        reader.readAsDataURL(file.raw)
        reader.onload = ()=> {
          let _base64 = reader.result
          let BASE64 = _base64.split(',')
          this.base64_img.push({
            name: 'base64图片',
            url: BASE64[1]
          })
          console.log(this.base64_img);
        }
        this.ruleForm['imgs'].push({
          name: '文件',
          url: file.url
        })
        console.log(this.ruleForm['imgs']);
        
      },
      handleRemove(file, fileList) {
        this.ruleForm.imgs.map((item, index) => {
          if (item.uid === file.uid) {
            this.ruleForm.imgs.splice(index, 1)
          }
        })
      },
      handlePictureCardPreview(file) {
        this.dialogImageUrl = file.url;
        this.dialogVisible = true;
      }
    },
    mounted () {
      console.log(this.user);
      
    },
    created () {
      // this.$axios.post('/users/getUser')
      this.$axios.get('/users/getUser').then(res=>{
        this.user = res.data.user;
        // console.log(res.data.user);
        
      })
      
    }
  }

</script>

<style lang="scss" scoped>
  .container {
    .bread-nav {
      width: 1190px;
      height: 60px;
      display: flex;
      flex-direction: row;
      justify-content: flex-start;
      align-items: center;

      .bread-nav-first {
        display: inline-block;
        width: 54px;
        height: 60px;
        line-height: 60px;
        font-weight: 700px;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;

        i {
          color: #606266;
        }
      }

      .bread-nav-second {
        margin-left: 5px;
        display: inline-block;
        color: #606266;
        width: 54px;
        height: 60px;
        line-height: 60px;
      }
    }

    .content {
      width: 1190px;
      background: #fff;
      padding: 40px 290px 60px 60px;
      box-sizing: border-box;

      /deep/ .el-form {
        width: 840px;
      }
    }
  }

</style>
