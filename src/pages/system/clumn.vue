<template >
    <div>
        
        <el-button type="success" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small">删除</el-button>
        <el-table :data="clumns">
            <el-table-column label="编号" prop="id" fixed="left"></el-table-column>
            <el-table-column label="栏目名称" prop="name" fixed="left"></el-table-column>
            <el-table-column label="序号" prop="number"></el-table-column>
            <el-table-column label="父栏目" prop="fartherClumn" width="120"></el-table-column>
            <el-table-column fixed="right" label="操作" >
                <template v-slot="slot">
                    
                    <a href="" @click.prevent="toDeleteHandler (slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUpdateHandler (slot.row)">修改</a>
                </template>
            </el-table-column>
        </el-table>
        <el-table-column width=30><el-checkbox>
            </el-checkbox>
            </el-table-column>

        <el-dialog title="添加栏目信息" :visible.sync="visible" width="60%">
             <el-form label-width="80px">
                 <el-form-item label="栏目名称">
                       <el-input v-model="form.name"></el-input>
                 </el-form-item>
            <el-form-item label="序号">
                <el-input type="number" v-model="form.number"></el-input>
            </el-form-item>
             </el-form>
             <span slot="footer" class="dialog-footer">
         <el-button @click=" closeModule" size="small">取 消</el-button>
         <el-button type="primary" @click="closeModule" >确 定</el-button>
        </span>
        </el-dialog>
    </div>
</template>
<script>
import request from '@/utils/request'
export default {
   methods:{
       submitHandler(){
         let url="http://localhost:6677/category/saveOrUpdate"
            //从前端向后台发送请求，完成数据的保存操作
            request({
                url,
                method:"POST",
                //告诉给后台我的请求中放的是查询字符
                headers:{
                    "Content-Type":"application/x-www-form-urlencoded"
                },
                //请求题中的数据，将this.form转换为查询字符串发送给后台
                data:querystring.stringify(this.form)
            }).then((response)=>{
                //请求结束，模态框关闭
                this.closeModule();
                //刷新
                this.loadData();
                //提示消息
                this.$message({
                    type:"success",
                    message:response.message
                })
            })
      },
       toAddHandler(){
           this.visible="ture"
       },
       loadData(){
            let url = "http://localhost:6677/category/findAll"
             request.get(url).then((response)=>{
             this.clumns = response.data;
          })
       },
       toUpdateHandler(row){
        this.title="修改栏目信息",
        this.visible=true;
      },
      toAddHandler(){
          this.title="添加栏目信息"
          this.visible=true;
      },
      closeModule(){
          this.visible=false;
      },
      toDeleteHandler(id){
          
        this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        })
      
      }
   },
   data(){
       return{
            visible:false,
            clumns:[],
            form:{
                type:"clumn"
            }
       }
    },

     created(){
      this.loadData();
    }
   } 


</script>
<style  scoped>

</style>