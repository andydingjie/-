<template>
  <div>

    <api-task
        :type="opsType"
        :apiProjectId="currentApiProjectId"
        v-if="showApiTask">
    </api-task>
    <div v-else>
      <div class="crumbs">
        <el-breadcrumb separator="/">
          <el-breadcrumb-item>
            <i class="el-icon-lx-calendar"></i> API任务管理
          </el-breadcrumb-item>
          <el-breadcrumb-item>API项目</el-breadcrumb-item>
        </el-breadcrumb>
      </div>
      <el-table
          :data="tableData"
          style="width: 100%">
        <el-table-column
            prop="id"
            label="id"
            sortable
            width="180">
        </el-table-column>
        <el-table-column
            prop="api_project_name"
            label="API项目名称"
            sortable
            width="180">
        </el-table-column>
        <el-table-column
            prop="describe"
            label="项目描述"
            width="180">
        </el-table-column>
        <el-table-column
            prop="updata_time"
            label="更新时间"
            width="180">
        </el-table-column>

        <el-table-column
            prop="create_time"
            label="创建时间"
            width="180">
        </el-table-column>
        <el-table-column label="操作">
          <template slot-scope="scope">
            <el-button
                size="mini"
                @click="enterApiProjectTestCase(scope.row)">进入</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>

<script>
import {getApiProjectRequest} from '../../../api/apiProject';
import ApiTask from './ApiTask';
export default {
  name: 'ApiProjectTask',
  data() {
    return {
      tableData: [],

      page: {
        page: 1,
        total: 0,
        size: 10,

      },

      projectId: "",

      opsType: "enter",

      showApiTask: false,
      // 当前项目ID
      currentApiProjectId: 0,



    }
  },
  methods: {


    handleEdit(index, row) {
      console.log(index, row);
    },
    handleDelete(index, row) {
      console.log(index, row);
    },
    handleSizeChange(size) {
      this.page.size = size;
      this.getApiProjectFun()
    },
    handleCurrentChange(index) {
      this.page.page = index;
      this.getApiProjectFun()
    },

    //获取所有项目列表
    getApiProjectFun() {
      let req = {
        page: this.page.page,
        size: this.page.size,
      };
      getApiProjectRequest(req).then(data => {
        console.log(data);
        if (true === data.data.success) {
          this.tableData = data.data.data;
          this.page.total = data.data.data.total;
          this.page.page = data.data.data.page;
          this.page.size = data.data.data.size;

        } else {
          this.$message.error("获取接口失败")
        }
      })
    },


    //进入项目页面
    enterApiProjectTestCase(data){

      this.opsType = 'enter';
      this.currentApiProjectId = data.id;

      this.showApiTask = true;

    },
    // 返回项目列表
    returnProject(){
      this.showApiTask = false;

    },

  },


  components: {
    'api-task': ApiTask
  },


  created() {
    this.getApiProjectFun();
  }




};
</script>

<style scoped>

</style>