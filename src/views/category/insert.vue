<template>
    <div>
        <el-divider></el-divider>
        <el-form :model="form" ref="form" :rules="rules" label-width="80px">
            <el-form-item label="名称" prop="cname" >
                <el-input prefix-icon="" v-model="form.cname" placeholder="cname" clearable>
                    <i slot="prefix" class="el-input-icon el-icon-user-solid"></i>
                </el-input>
            </el-form-item>
            <el-form-item label="缩略图" prop="thumb" >
                <el-input prefix-icon="" v-model="form.thumb" placeholder="thumb" clearable>
                    <i slot="prefix" class="el-input-icon el-icon-user-solid"></i>
                </el-input>
            </el-form-item>
            <el-form-item label="排序"  prop="sort" >
                <el-input prefix-icon="" v-model="form.sort" placeholder="sort" clearable>
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
    import {HOSTNAME} from "../../config/base";
    export default {
        name: "add",
        data: function () {
            return {
                form: {
                    cname: '',
                    sort: '',
                    thumb:''
                },
                rules: {
                    cname: [
                        {required: true, message: '请输入商品分类名称', trigger: 'blur'},
                        {min: 2, max: 4, message: '长度在 2 到 4 个字符', trigger: 'blur'}
                    ],
                    thumb: [
                        {required: true, message: '请上传商品缩略图'},
                    ],
                    sort: [
                        {required: true, message: '请输入商品排序', trigger: 'number'},
                        {min: 2, max: 10, message: '长度在 2 到 10 个数字', trigger: 'number'}
                    ],
                },
            }

        },
        methods: {
            submit() {
                fetch(HOSTNAME+'/api/category', {
                    method: 'POST', // *GET, POST, PUT, DELETE, etc.
                    body: JSON.stringify(this.form),
                    headers: new Headers({
                        'Content-Type': 'application/json'
                    })
                })
                    .then(res => res.json())
                    .then(data => {
                        if (data.code == 200) {
                            this.$message.success(data.msg);
                            this.$refs.form.resetFields();
                        } else {
                            this.$message.error(data.msg)
                        }
                    })
            }
        }
    }

</script>

<style scoped>
    .form {
        width: 500px;
    }

</style>