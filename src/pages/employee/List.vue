<template>
    <div>
        <el-button type="success" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small">删除</el-button>

        <el-table :data="employees">
            <el-table-column label="编号" prop="id" fixed="left"></el-table-column>
            <el-table-column label="姓名" prop="realname" fixed="left"></el-table-column>
            <el-table-column label="性别" prop="gender"></el-table-column>
            <el-table-column label="身份证号" prop="idCard" width="200" ></el-table-column>
            <el-table-column label="银行卡号" prop="bankCard" widtn="200"></el-table-column>
            <el-table-column label="联系方式" prop="telephone" width="120"></el-table-column>
            <el-table-column fixed="right" label="操作" >
                <template v-slot="slot">
                    
                    <a href="" @click.prevent="toDeleteHandler (slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUpdateHandler (slot.row)">修改</a>
                </template>
            </el-table-column>
        </el-table>
        <!--分也开始-->
      <el-pagination
       layout="prev, pager, next"
       :total="50">
      </el-pagination>
       <!--分页结束-->
       <!--模态框-->
       <el-dialog 
       title="录入员工信息" :visible.sync="visible" width="60%">
         <el-form label-width="80px">
            <el-form-item label="真实姓名">
                 <el-input v-model="form.realname"></el-input>
            </el-form-item>
            <el-form-item label="密码">
                <el-input type="password" v-model="form.password"></el-input>
            </el-form-item>
            <el-form-item label="手机号">
                <el-input v-model="form.telephone"></el-input>
            </el-form-item>
            <el-form-item label="身份证号">
                <el-input v-model="form.idCard"></el-input>
            </el-form-item>
            <el-form-item label="银行卡号">
                <el-input v-model="form.bankCard"></el-input>
            </el-form-item>
            <el-form-item label="性别">
             <el-radio-group v-model="form.gender">
                <el-radio label="男">男</el-radio>
                <el-radio label="女">女</el-radio>
             </el-radio-group>
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
import querystring from 'querystring'
export default {
    methods:{
      //提交
      submitHandler(){
         let url="http://localhost:6677/waiter/saveOrUpdate"
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
                this.closeModalHandler();
                //刷新
                this.loadData();
                //提示消息
                this.$message({
                    type:"success",
                    message:response.message
                })
            })
      },
      //重载员工数据
     loadData(){
            let url = "http://localhost:6677/waiter/findAll"
             request.get(url).then((response)=>{
        // 将查询结果设置到customers中，this指向外部函数的this
             this.employees = response.data;
          })
       },
      toUpdateHandler(row){
        this.title="修改员工信息",
        this.visible=true;
      },
      toAddHandler(){
          this.title="添加员工信息"
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
    
    //用于存放要向网页中显示的数据
    data(){
      return{
        title:"录入员工信息",
        visible:false,
        employees:[],
        form:{
          type:"waiter"
        }
      }
        
    },
    created(){
      this.loadData();
    }
}
</script>

<style scoped>

</style>