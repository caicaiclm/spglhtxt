<template>
  <el-form ref="form" :model="form" label-width="80px">
    <el-form-item label="所属类别">
      <el-select v-model="form.category_id" placeholder="请选择">
        <el-option :label="item.title" :value="item.category_id" v-for="(item, index) in categorys" :key="index"></el-option>
      </el-select>
    </el-form-item>    
    <el-form-item label="是否发布">
      <el-switch v-model="form.status"></el-switch>
    </el-form-item>
    <el-form-item label="推荐类型">
      <el-checkbox label="置顶" v-model="form.is_top"></el-checkbox>
      <el-checkbox label="热门" v-model="form.is_hot"></el-checkbox>
    </el-form-item>   
    <el-form-item label="内容标题">
      <el-input v-model="form.title"></el-input>
    </el-form-item>
    <el-form-item label="副标题">
      <el-input v-model="form.sub_title"></el-input>
    </el-form-item>
    <el-form-item label="封面图片">
      <el-upload
        class="avatar-uploader"
        action="http://localhost:8899/admin/article/uploadimg"
        :show-file-list="false"
        :on-success="handleAvatarSuccess"
        :before-upload="beforeAvatarUpload">
        <img v-if="imageUrl" :src="imageUrl" class="avatar">
        <i v-else class="el-icon-plus avatar-uploader-icon"></i>
      </el-upload>
    </el-form-item> 
    <el-form-item label="商品货号">
      <el-input v-model="form.goods_no"></el-input>
    </el-form-item>
    <el-form-item label="库存数量">
      <el-input v-model="form.stock_quantity"></el-input>
    </el-form-item>
    <el-form-item label="市场价格">
      <el-input v-model="form.market_price"></el-input>
    </el-form-item>
    <el-form-item label="销售价格">
      <el-input v-model="form.sell_price"></el-input>
    </el-form-item>
    <el-form-item label="图片相册">
      <div>
        <el-upload
          action="http://localhost:8899/admin/article/uploadimg"
          list-type="picture-card"
          :on-preview="handlePictureCardPreview"
          :on-remove="handleRemove"
          :on-success="handePicSuccess">
          <i class="el-icon-plus"></i>
        </el-upload>
        <el-dialog :visible.sync="dialogVisible">
          <img width="100%" :src="dialogImageUrl" alt="">
        </el-dialog> 
      </div>      
    </el-form-item>    
    <el-form-item label="内容摘要">
      <el-input type="textarea" v-model="form.zhaiyao"></el-input>
    </el-form-item>
    <el-form-item label="内容描述">
      <!-- <el-input type="textarea" v-model="form.desc"></el-input> -->
      <quillEditor v-model="form.content"></quillEditor>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="onSubmit">立即创建</el-button>
      <el-button>取消</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
  // 导入富文本编辑器的样式
  import 'quill/dist/quill.core.css'
  import 'quill/dist/quill.snow.css'
  import 'quill/dist/quill.bubble.css'
  // 导入组件
  import {quillEditor} from 'vue-quill-editor'
export default {
  data() {
    return {
      form: {
        category_id: '',
        status: false,
        is_top: false,
        is_hot: false,
        title: '',
        sub_title: '',
        imgList: [],
        goods_no: '',
        stock_quantity: '',
        market_price: '',	
        sell_price: '',
        fileList: [],
        zhaiyao: '',
        content: '',
        is_slide: false
      },
      categorys: [],
      //封面图片
      imageUrl: '',
      //图片相册
      dialogImageUrl: '',
      dialogVisible: false
    };
  },
  components: {
    quillEditor
  },
  mounted() {
    this.$axios({
      url: "/admin/category/getlist/goods"
    }).then(res => {
      // console.log(res);
      const {message} = res.data;
      this.categorys = message;
    })
  },
  methods: {
    onSubmit() {
      // console.log(this.form);
      // console.log(this.form.content);
      this.$axios({
        url: '/admin/goods/add/goods',
        method: 'POST',
        data: this.form,
        withCredentials: true
      }).then(res => {
        console.log(res);
        const {status, message} = res.data;
        if(status === 0){
          // 弹出提示信息
          this.$message({
            type: 'success',
            message
          })
          //返回上一页
          this.$router.back();
        }
      })
    },
    handleAvatarSuccess(res, file) {
        //图片预览
        this.imageUrl = URL.createObjectURL(file.raw);
        // 成功之后赋值给form.imgList
        // console.log(res, file);
        this.form.imgList = [res];
      },
      beforeAvatarUpload(file) {
        const isJPG = file.type === 'image/jpeg';
        const isLt2M = file.size / 1024 / 1024 < 2;

        if (!isJPG) {
          this.$message.error('上传头像图片只能是 JPG 格式!');
        }
        if (!isLt2M) {
          this.$message.error('上传头像图片大小不能超过 2MB!');
        }
        return isJPG && isLt2M;
      },
      handleRemove(file, fileList) {                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          
        console.log(file, fileList);
        this.form.fileList = fileList.map(v => {
          return v.response;
        })
      },
      handePicSuccess(res, file, fileList){
        // console.log(res, file, fileList);
        this.form.fileList = fileList.map(v => {
          return v.response;
        })
      },
      handlePictureCardPreview(file) {
        this.dialogImageUrl = file.url;
        this.dialogVisible = true;
      }
  }
};
</script>

<style>
.avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }
  .avatar-uploader .el-upload:hover {
    border-color: #409EFF;
  }
  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
  }
  .avatar {
    width: 178px;
    height: 178px;
    display: block;
  }
</style>
