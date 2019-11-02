<template>
    <div>
        <template>
            <el-table
                    :data="fruitArr"
                    border
                    style="width: 100%">
                <el-table-column
                        prop="id"
                        label="ID"
                        width="80">
                </el-table-column>
                <el-table-column
                        prop="cname"
                        label="分类名称"
                        width="80">
                </el-table-column>
                <el-table-column
                        prop="thumb"
                        label="缩略图"
                        width="80">
                    <template slot-scope="scope">
                        <img style="width: 100px" :src="imghost+scope.row.thumb" title="scope.row.cname" alt="scope.row.cname">
                    </template>
                </el-table-column>
                <el-table-column
                        prop="sort"
                        label="排序"
                        width="80">
                </el-table-column>
                <el-table-column
                        prop="create_time"
                        label="上架时间"
                        width="180">
                </el-table-column>
                <el-table-column
                        prop="update_time"
                        label="修改时间"
                        width="180">
                </el-table-column>
                <el-table-column
                        label="操作"
                        width="200">
                    <template slot-scope="scope">
                      <router-link :to="{name:'cateedit',query:{id:scope.row.id}}" tag="el-button">编辑</router-link>
                        <el-button
                                @click="deleteCate(scope.row.id)">
                            Delete
                        </el-button>
                    </template>
                </el-table-column>
            </el-table>
        </template>
    </div>
</template>

<script>
    import {HOSTNAME,IMGHOSTNAME,SUCCESS} from "../../config/base";
    export default {

        name: "query",
        data() {
            return {
                fruitArr: [],
                total: 0,
                limit:2,
                form:{
                    content:'',
                    ctime:'',
                },
               imghost:IMGHOSTNAME,
            }
        },
        methods: {
            getdaily() {
                fetch( HOSTNAME+'/api/category')
                    .then(res => res.json())
                    .then(data => {
                        if (data.code == SUCCESS) {
                            this.fruitArr = data.data;
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
            deleteCate(id){
                let token=sessionStorage.getItem('token');
                fetch(HOSTNAME+'/api/category/'+id,{
                    method:'DELETE',
                    headers:{
                        'token':token
                    },
                }) .then(res => res.json())
                .then(data => {
                    if (data.code == SUCCESS) {
                        this.fruitArr=this.fruitArr.filter(ele=>ele.id!=id);
                    } else {
                        this.$message.error(data.msg)
                    }
                })


            }

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