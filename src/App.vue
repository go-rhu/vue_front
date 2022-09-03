

<template>
<div class="table-box">
    <div class="title">
        <h2>简单的CURD</h2>
    </div>
    <div class="query-box">
        <el-input class="query-Input" v-model="qryInput" placeholder="请输入姓名搜索" />
    <div class="btn_list">
      <el-button type="danger" @click="handleDelList" v-if="multipleSelection.length>0">删除多选</el-button>
      <el-button type="primary" @click="handleAdd">增加</el-button>
    </div>
    </div>
<!--table-->
<el-table  border
ref="multipleTableRef"
:data="tableData" 
style="width: 100%"
@selection-change ="handleSelectionChange"
>
    <el-table-column type="selection" width="55" />
    <el-table-column prop="name" label="姓名" width="120" />
    <el-table-column prop="email" label="邮箱" width="120" />
    <el-table-column prop="phone" label="电话" width="120" />
    <el-table-column prop="state" label="状态" width="120" />
    <el-table-column prop="address" label="地址" width="300" />

    <el-table-column fixed="right" label="操作" width="120">
      <template #default="scope">
        <el-button link type="primary" size="small" @click="handleDelOne(scope.row)" style="color:red">删除</el-button
        >
        <el-button link type="primary" size="small" @click="handleEdit(scope.row)">编辑</el-button>
      </template>
    </el-table-column>
  </el-table>
  <el-dialog v-model="dialogFormVisible" :title="dialogType==='add'?'新增':'编辑'">
    <el-form :model="tableForm">
      <el-form-item label="姓名" :label-width="100">
        <el-input v-model="tableForm.name" autocomplete="off" />
      </el-form-item>
      <el-form-item label="邮箱" :label-width="100">
        <el-input v-model="tableForm.email" autocomplete="off" />
      </el-form-item>
      <el-form-item label="电话" :label-width="100">
        <el-input v-model="tableForm.phone" autocomplete="off" />
      </el-form-item>
      <el-form-item label="状态" :label-width="100">
        <el-input v-model="tableForm.state" autocomplete="off" />
      </el-form-item>
      <el-form-item label="地址" :label-width="100">
        <el-input v-model="tableForm.address" autocomplete="off" />
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button type="primary" @click="dialogConfirm"
          >确认</el-button
        >
      </span>
    </template>
  </el-dialog>
</div>

</template>

<script setup>
    //**数据
    import{ref} from "vue";
    //用了$ref后不能watch监听
    let qryInput =$ref("");
    let tableData =$ref([
      {
    id: '1',
    name: 'Tom',
    email:'123@qq.com',
    phone:'13800138000',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id: '2',
    name: 'Tom2',
    email:'123@qq.com',
    phone:'13800138000',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id: '3',
    name: 'Tom3',
    email:'123@qq.com',
    phone:'13800138000',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id: '4',
    name: 'Tom4',
    email:'123@qq.com',
    phone:'13800138000',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },])
    let multipleSelection = $ref([]) 
    let dialogFormVisible =$ref(false)
    let tableForm=$ref({
      name:"张三",
      email:"123@qq.com",
      phone:"1380013800",
      state:"在职",
      address:"广东省",
    })
    let dialogType=$ref("add")
    //**方法
    //删除一条
    const handleDelList=()=>{
      multipleSelection.forEach(
        id=>{
          handleDelOne({id})
        })
        multipleSelection=[]
    }
    //删除一条
    const handleDelOne=({id})=>{
        console.log(id)
        //1.通过ID获取条目对应的索引值
        let index =tableData.findIndex(item=>item.id===id)
        console.log(index)
        //2.通过索引值删除对应的条目
        tableData.splice(index,1)
    }
    //选中一条
    const handleSelectionChange=(val)=>{
      //multipleSelection=val
      //console.log(val)
      multipleSelection=[]
      val.forEach(item=>{
        multipleSelection.push(item.id)
      })
      console.log(multipleSelection)
    }
    //新增一行准备
    const handleAdd=()=>{
      dialogFormVisible =true
      tableForm = {}
      dialogType="add"
      console.log(dialogFormVisible)
    }
    //确认增加一行数据
    const dialogConfirm=()=>{
      dialogFormVisible = false
      //1.拿到数据
      //2.将数据写到table
      if (dialogType==='add'){
      tableData.push({
        ID:(tableData.lenght+1).toString(),
      ...tableForm})
      console.log(tableData)
      }else{
        //获取到当前的索引，替换当前索引值对应的数据
        let index=tableData.findIndex(item=>item.id===tableForm.id)
        console.log(index);
        tableData[index]=tableForm
      }
    }
    const handleEdit=(row)=>{
      //显示弹窗
      dialogFormVisible = true
      //更改全局变量
      dialogType="edit"
      //行赋值
      tableForm={...row}
    }

</script>
<style scoped >
    /*position:absolute;
    top:50%;
    left:50%;
    transform:translate(-50%,-50%);*/
.table-box{
    width:800px;
    margin:200px auto;/*修改块状背景*/
}
.title{
    text-align:center;
}
.query-box{
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
}
.query-Input{
    width:200px
}
</style>
