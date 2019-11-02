<template>
    <div :v-if="form">
        <el-divider></el-divider>
        <el-form  :model="form" ref="form" :rules="rules" label-width="80px">
            <el-form-item label="名称" prop="cname" >
                <el-input prefix-icon="" v-model="form.cname" placeholder="cname" clearable>
                    <i slot="prefix" class="el-input-icon el-icon-user-solid"></i>
                </el-input>
            </el-form-item>
            <el-form-item label="缩略图" prop="thumb" >
                <el-upload
                        v-model="form.thumb"
                        :action="uploadurl"
                        :on-success="uploadSuccess"
                >
                    <img v-if="imageUrl" :src="imageUrl" class="avatar">
                    <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                </el-upload>
            </el-form-item>
            <el-form-item label="排序"  prop="sort" >
                <el-input prefix-icon="" v-model.number="form.sort" placeholder="sort" clearable>
                    <i slot="prefix" class="el-input-icon el-icon-user-solid"></i>
                </el-input>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="submit('form')">提交</el-button>
                <el-button >取消</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>

<script>
    import {HOSTNAME,SUCCESS,IMGHOSTNAME,TOKENFAIL,FAIL} from "../../config/base";
    export default {
        name: "edit",
        data: function () {
            return {
                uploadurl:HOSTNAME+'/api/upload',
                imageUrl:'',
                rules: {
                    cname: [
                        {required: true, message: '请输入商品分类名称', trigger: 'blur'},
                        {min: 2, max: 4, message: '长度在 2 到 4 个字符', trigger: 'blur'}
                    ],
                    thumb: [
                        {required: true, message: '请上传商品缩略图'},
                    ],
                    sort: [
                        {required: true, message: '请输入商品排序', trigger: 'blur'},
                        {min: 2, max: 3, message: '长度在 2 到 3 个数字'}
                    ],
                },
                form:{
                    cname:'',
                    sort:'',
                    thumb:'',
                }
            }

        },
        methods: {
            Getcate(id){
                fetch(HOSTNAME+'/api/category/'+id,{
                    method:'GET',
                    headers:new Headers({
                        'Content-Type':'application/json',
                        'token':sessionStorage.getItem('token')
                    })
                })
                    .then(res => res.json())
                    .then(data => {
                        let {code,msg}=data
                        if (code == TOKENFAIL) {
                            this.$message({
                                type:'warning',
                                message:msg
                            })
                        } else if (code==SUCCESS) {
                            this.$message({
                                type:'success',
                                message:msg
                            })
                            this.form=data.data;
                            this.imageUrl=IMGHOSTNAME+data.data.thumb;
                        }else if (code==FAIL) {
                            this.$message({
                                type: 'warning',
                                message:msg
                            })
                        }
                    })
            },
            submit() {
                let token=sessionStorage.getItem('token')
                fetch(HOSTNAME+'/api/category/'+this.id, {
                    method: 'PUT', // *GET, POST, PUT, DELETE, etc.
                    body: JSON.stringify(this.form),
                    headers: new Headers({
                        'Content-Type': 'application/json',
                        'token':token
                    })
                })
                    .then(res => res.json())
                    .then(data => {
                        let {code ,msg}=data;
                        if (code==TOKENFAIL){
                            this.$message({
                                type:'warning',
                                message:msg
                            })
                        } else if (code==SUCCESS){
                            this.$message.success(data.msg);
                            this.$refs.form.resetFields();
                            window.console.log(data.data)
                        }
                        else if (code == FAIL) {
                            this.$message({
                                type: 'warning',
                                message:msg
                            })
                        }
                    })
            },
            uploadSuccess(res) {
                this.imageUrl =  IMGHOSTNAME+res.src;
                this.form.thumb=res.src;
                window.console.log(res);
            },
        },
        beforeMount(){
            this.id=this.$route.query.id
            this.Getcate(this.id)
        }
    }

</script>
<style>
    .el-upload {
        border: 1px dashed #d9d9d9;}
</style>
<style scoped>
    .form {
        width: 500px;
    }
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