<template>
    <div class="app-container">
        <div style="width:100%;margin-bottom: 10px;">
            <el-input v-model="input" placeholder="请输入关键词" size="small" style="width:200px;margin-right: 10px;">
            </el-input>
            <el-button style="margin-right: 10px;" type="primary" size="small">搜索</el-button>
            <el-button icon="el-icon-edit" type="default" size="small" @click="dialogFormVisible = true">添加</el-button>
        </div>
        <el-table style="height:700px" v-loading="listLoading" :data="list" element-loading-text="Loading" fit
            highlight-current-row>
            <el-table-column align="center" label="序号" width="95">
                <template slot-scope="scope">
                    {{ scope.$index + 1 }}
                </template>
            </el-table-column>
            <el-table-column label="内容">
                <template  slot-scope="scope" >
                     <router-link to="/home">{{ scope.row.title }}</router-link>
                </template>
            </el-table-column>
            <el-table-column label="用户" width="110" align="center">
                <template slot-scope="scope">
                    <span>{{ scope.row.author }}</span>
                </template>
            </el-table-column>
            <el-table-column label="评论文章" width="110" align="center">
                <template slot-scope="scope">
                    {{ scope.row.pageviews }}
                </template>
            </el-table-column>
            <el-table-column class-name="status-col" label="审核状态" width="110" align="center">
                <template slot-scope="scope">
                    <el-tag :type="scope.row.status | statusFilter">{{
                            scope.row.status
                    }}</el-tag>
                </template>
            </el-table-column>
            <el-table-column align="center" prop="created_at" label="评论时间" width="200">
                <template slot-scope="scope">
                    <i class="el-icon-time" />
                    <span>{{ scope.row.display_time }}</span>
                </template>
            </el-table-column>

            <el-table-column align="center" prop="created_at" label="操作" width="100">
                <template>
                    <el-select size="small" v-model="value" placeholder="编辑" @change="Dopreate">
                        <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value">
                        </el-option>
                    </el-select>
                </template>
            </el-table-column>
        </el-table>
        <div style=" height: 50px; line-height: 50px; margin-top: 10px;">
            <el-pagination background @size-change="handleSizeChange" @current-change="handleCurrentChange"
                :current-page="currentPage" :page-sizes="[10, 20, 30, 40]" :page-size="pageSize"
                layout="total, sizes, prev, pager, next, jumper" :total="total">
            </el-pagination>
        </div>

        <div>
            <el-dialog title="收货地址" :visible.sync="dialogFormVisible" width="30%">
                <el-form :model="form">
                    <el-form-item label="内容" :label-width="formLabelWidth">
                        <el-input v-model="form.name" autocomplete="off" width="50px"></el-input>
                    </el-form-item>
                    <el-form-item label="用户" :label-width="formLabelWidth">
                        <el-input v-model="form.name" autocomplete="off"></el-input>
                    </el-form-item>
                     <el-form-item label="评论文章" :label-width="formLabelWidth">
                        <el-input v-model="form.name" autocomplete="off"></el-input>
                    </el-form-item>
                    <el-form-item label="状态" :label-width="formLabelWidth">
                        <el-radio v-model="radio" label="1">发布</el-radio>
                        <el-radio v-model="radio" label="2">违规</el-radio>
                    </el-form-item>
                </el-form>
                <div slot="footer" class="dialog-footer">
                    <el-button @click="dialogFormVisible = false">取 消</el-button>
                    <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
                </div>
            </el-dialog>

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
            currentPage: 1,
            pageSize: 10,
            total: 0,
            input: '',
            options: [
                {
                    value: 1,
                    label: '编辑'
                },
                {
                    value: 0,
                    label: '删除'
                }
            ],
            value: 1,
            dialogFormVisible: false,
            form: {
                name: '',
                region: '',
                date1: '',
                date2: '',
                delivery: false,
                type: [],
                resource: '',
                desc: ''
            },
            formLabelWidth: '100px',
            radio: '1'
        };
    },
    created() {
        this.fetchData(this.currentPage, this.pageSize);
    },
    methods: {
        fetchData(curpage = this.currentPage, pageSize = this.pageSize) {
            this.listLoading = true;
            getList().then((response) => {
                let start = (curpage - 1) * pageSize
                let end = curpage * pageSize
                this.list = response.data.items.slice(start, end);
                this.total = response.data.items.length
                this.listLoading = false;
            });
        },
        handleSizeChange(val) {
            //对数据请求渲染  每页多少条
            this.pageSize = val
            this.fetchData()
        },
        handleCurrentChange(val) {
            this.currentPage = val
            this.currentPage = val
            //切换当前页，重新渲染页面
            this.fetchData()


        },
        //操作
        Dopreate(e) {
            console.log('操作选择项:', e);
        },

        toDetail(){
            console.log('跳转页面');
        }
    },
};
</script>
