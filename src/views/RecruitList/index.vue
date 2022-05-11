<template>
    <div class="app-container">
        <el-table style="height:600px" v-loading="listLoading" :data="list" element-loading-text="Loading" border fit highlight-current-row>
            <el-table-column align="center" label="ID" width="95">
                <template slot-scope="scope">
                    {{ scope.$index +1 }}
                </template>
            </el-table-column>
            <el-table-column label="文章标题">
                <template slot-scope="scope">
                    {{ scope.row.title }}
                </template>
            </el-table-column>
            <el-table-column label="作者" width="110" align="center">
                <template slot-scope="scope">
                    <span>{{ scope.row.author }}</span>
                </template>
            </el-table-column>
            <el-table-column label="评论" width="110" align="center">
                <template slot-scope="scope">
                    {{ scope.row.pageviews }}
                </template>
            </el-table-column>
            <el-table-column class-name="status-col" label="状态" width="110" align="center">
                <template slot-scope="scope">
                    <el-tag :type="scope.row.status | statusFilter">{{
                            scope.row.status
                    }}</el-tag>
                </template>
            </el-table-column>
            <el-table-column align="center" prop="created_at" label="发布时间" width="200">
                <template slot-scope="scope">
                    <i class="el-icon-time" />
                    <span>{{ scope.row.display_time }}</span>
                </template>
            </el-table-column>
        </el-table>
        <div style=" height: 50px; line-height: 50px; margin-top: 10px;">
            <el-pagination background @size-change="handleSizeChange" @current-change="handleCurrentChange"
                :current-page="currentPage" :page-sizes="[5,10,15,20]" :page-size="pageSize"
                layout="total, sizes, prev, pager, next, jumper" :total="total">
            </el-pagination>

        </div>

    </div>
</template>

<script>
import { getList } from "@/api/table";

export default {
    filters: {
        statusFilter(status) {
            const statusMap = {
                published: "success",
                draft: "gray",
                deleted: "danger",
            };
            return statusMap[status];
        },
    },
    data() {
        return {
            list: null,
            listLoading: true,
            currentPage:1,
            pageSize:5,
            total:0
        };
    },
    created() {
        this.fetchData(this.currentPage,this.pageSize);
    },
    methods: {
        fetchData(curpage=this.currentPage,pageSize = this.pageSize) {
            console.log('修改页面:',curpage,pageSize);
            console.log('pageSize：',pageSize);
            this.listLoading = true;
            getList().then((response) => {
                let start = (curpage-1)*pageSize
                let end = curpage *pageSize
                console.log('页面数据',curpage,start,end);
                this.list = response.data.items.slice(start,end);
                this.total = response.data.items.length
                this.listLoading = false;
            });
        },
        handleSizeChange(val) {
            //对数据请求渲染  每页多少条
            console.log(`每页 ${val} 条`);
            this.pageSize = val
            this.fetchData()
        },
        handleCurrentChange(val) {
            this.currentPage = val
            console.log(`当前页: ${val}`);
            this.currentPage = val
            //切换当前页，重新渲染页面
            this.fetchData()


        }
    },
};
</script>
