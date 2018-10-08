<template>
	<div>
		<h3>资料修改</h3>
			<el-form ref="form" :model="form" label-width="80px">
			  <el-form-item label="我的角色">
					<el-input v-model="form.part" class="part" disabled></el-input>

					<!-- <el-select v-model="form.part">
				  <el-option label="超级管理员" value="超级管理员" disabled></el-option>
				  <el-option label="管理员" value="管理员"></el-option> 
					</el-select>  -->
			  </el-form-item>

				<el-form-item label="用户名" class="xg">
					<el-input v-model="form.username" disabled></el-input>
			  </el-form-item>

			  <el-form-item label="昵称" class="xg">
			  	<el-input v-model="form.nickname"></el-input>
			  </el-form-item>

			  <el-form-item label="性别">
					<el-radio-group v-model="form.radio">
						<el-radio v-model="form.radio" :label="form.value1"></el-radio>
						<el-radio v-model="form.radio" :label="form.value2"></el-radio>
					</el-radio-group>
			  </el-form-item>
				
			  <el-form-item label="头像" class="xg" >
				  
				  <el-upload
						class="upload-demo"
						ref="upload"
						action="/api/chk/"
						:on-preview="handlePreview"
						:on-remove="handleRemove"
						:file-list="fileList"
						:auto-upload="false">
						<el-button slot="trigger" size="small" type="primary">选取文件</el-button>
						<el-button style="margin-left: 10px;" size="small" type="success" @click="submitUpload">上传到服务器</el-button>
						<div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
					</el-upload>
						
			  </el-form-item>

			  <el-form-item label="手机" class="xg">
			  <el-input v-model="form.phone"></el-input>
			  </el-form-item>

			  <el-form-item label="邮箱" class="xg">
			  <el-input v-model="form.email"></el-input>
			  </el-form-item>

			  <el-form-item label="备注" class="bz">
			  <el-input
				  type="textarea"
				  :rows="3"
				  placeholder="请输入内容"
				  v-model="form.remark">
				</el-input>
				</el-form-item>

			  <el-form-item>
				<el-button  @click="onSubmit">确认修改</el-button>
				<el-button>重新填写</el-button>
			  </el-form-item>
		</el-form>
		<!-- <form action=http://10.3.137.38:8081/chk" method="post" enctype="multipart/form-data">
        a:<input type="text" name="a" />
        <hr>
        img1:<input type="file" name="img1" />
        <hr>
        <input type="submit" value="提交">
    </form> -->
	</div>
</template>
<script>
  export default {
    data() {
      return {
				fileList: [],
        form: {
          name: '',
          part: '',
          date1: '',
          date2: '',
          delivery: false,
          type: [],
          resource: '',
					desc: '',
					part:localStorage.getItem('part'),
					nickname:localStorage.getItem('nickname'),
					username:localStorage.getItem('user'),
					email:localStorage.getItem('email'),
					phone:localStorage.getItem('phone'),
					remark:localStorage.getItem('remark'),
					id:localStorage.getItem('id'),
				
					radio: '男',
					value1:'男',
					value2:'女'
        },
				textarea:''
      }
    },
    methods: {
			submitUpload() {
        this.$refs.upload.submit();
      },
      onSubmit() {
				this.$axios.get('/api/changename',{
					params:{
						nickname:this.form.nickname,
						male:this.form.radio,
						phone:this.form.phone,
						email:this.form.email,
						remark:this.form.remark,
						id:localStorage.getItem('id')
					}
				}).then(res=>{
					if(res.data == "修改成功"){
						this.$notify({
							title: '提示',
							message: '资料修改成功',
							offset: 100,
						});
					}
					console.log(res);
				})
        console.log('submit!');
      },
			// onChange(){
			// 	this.form.nickname:'',
			// 	this.form.radio:'',
			// 	this.form.phone:'',
			// 	this.form.email:'',
			// 	this.form.remark:''
			// },
			handleRemove(file, fileList) {
        console.log(file, fileList);
      },
      handlePreview(file) {
        console.log(file);
      },
			 handleExceed(files, fileList) {
        this.$message.warning(`当前限制选择 3 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
      },
			beforeRemove(file, fileList) {
        return this.$confirm(`确定移除 ${ file.name }？`);
      },
		}
  }
</script>
<style>
	.xg{
		width:300px;
	}
	.part{width:220px}
</style>