<template>
    <div>
        <template>
            <el-table
                    :data="dailyArr"
                    border
                    style="width: 100%">
                <el-table-column
                        prop="id"
                        label="ID"
                        width="80">
                </el-table-column>
                <el-table-column
                        prop="username"
                        label="姓名"
                        width="80">
                </el-table-column>
                <el-table-column
                        prop="sex"
                        label="性别"
                        width="80">
                </el-table-column>
                <el-table-column
                        prop="age"
                        label="年龄"
                        width="80">
                </el-table-column>
                <el-table-column
                        prop="num"
                        label="学号"
                        width="180">
                </el-table-column>
                <el-table-column
                        prop="tell"
                        label="电话"
                        width="180">
                </el-table-column>
                <el-table-column
                        prop="password"
                        label="密码"
                        width="180">
                </el-table-column>
                <el-table-column
                        prop="classes"
                        label="班级"
                        width="180">
                </el-table-column>
                <el-table-column
                        fixed="right"
                        label="操作"
                        width="100">
                    <template slot-scope="scope">
                      <router-link :to="{name:'studentedit',params:{id:scope.row.id}}" tag="el-button">编辑</router-link>
                        <el-button
                                @click="del(scope.row.id)">
                            Delete
                        </el-button>
                    </template>
                </el-table-column>
            </el-table>
        </template>
    </div>
</template>

<script>
    import {HOSTNAME} from "../../confing/base";
    export default {
        name: "query",
        data() {
            return {
                dailyArr: [],
                total: 0,
                limit:2,
                form:{
                    content:'',
                    ctime:'',
                }
            }
        },
        computed:{
            objTransformQS(){
                let str='';
                for (let i in this.form){
                    let val =this.form[i];
                    if (val != ''){
                        str+= '&'+i + '='+val
                    }
                }
                return str
            }
        },
        methods: {
            getdaily() {
                // let user = this.user.username;
                fetch(HOSTNAME + 'index/index/studentquery')
                    .then(res => res.json())
                    .then(data => {
                        if (data.code == 200) {
                            this.dailyArr = data.data;
                            this.total = data.count;
                            this.$message.success(data.msg);
                        } else {
                            this.$message.error(data.msg)
                        }
                    })
            },
            getForm() {
                this.user = JSON.parse(localStorage.user)
            },


        },
        //生命周期函数
        beforeMount() {
            this.getForm();
            this.getdaily();
        }
    }
</script>

<style scoped>

</style>