<template>
  <div class="app-container">
    <el-table
      v-loading="listLoading"
      :data="list"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
    >
      <el-table-column label="时间" align="center">
        <template slot-scope="scope">
          <i class="el-icon-time" />
          <span>{{ scope.row.sendtime }}</span>
        </template>
      </el-table-column>
      <el-table-column label="发送成功" align="center">
        <template slot-scope="scope">
          {{ scope.row.scount }}
        </template>
      </el-table-column>
      <el-table-column label="发送失败" align="center">
        <template slot-scope="scope">
          {{ scope.row.fcount }}
        </template>
      </el-table-column>

    </el-table>
    <pagination v-show="total>0" :total="total" :page.sync="listQuery.page" :limit.sync="listQuery.limit" style="text-align:center;" @pagination="fetchData" />
  </div>
</template>

<script>
import axios from 'axios'
import qs from 'qs'
import Pagination from '@/components/Pagination'

export default {
  components: { Pagination },
  data() {
    return {
      list: null,
      total: 0,
      listLoading: true,
      listQuery: {
        page: 1,
        limit: 24
      }
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      this.listLoading = true
      axios.get('http://api.798joy.com:18080/querybyhour?page=' + (this.listQuery.page - 1), qs.stringify()).then((response) => {
        if (response.data.code === '0') {
          this.list = response.data.data.logs
          this.total = response.data.data.pages * 24
        }
        this.listLoading = false
      }).catch(() => {
        this.listLoading = false
      })
    }
  }
}
</script>
