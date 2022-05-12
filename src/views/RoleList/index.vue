<template>
    <div>
        <el-table :data="data" style="width: 100%">
            <el-table-column prop="value" label="序号" width="width">
            </el-table-column>
            <el-table-column prop="name" label="角色名称" width="width">
            </el-table-column>
             <el-table-column prop="value" label="角色ID" width="width">
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
    </div>
</template>

<script>
export default {
    props: {

    },
    components: {

    },
    data() {
        return {
            data: [
                {
                    value: 1,
                    name: '求职责'
                },
                {
                    value: 2,
                    name: '招聘者'
                }
            ],
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
        }
    },
    methods: {
         //操作
        Dopreate(e) {
            console.log('操作选择项:', e);
        },
    }
}
</script>

<style lang="scss">
</style>