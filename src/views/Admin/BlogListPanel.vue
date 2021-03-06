<template>
    <div class="article-manage">
        <AdminHeader :pathList="pathList"></AdminHeader>
        <el-card class="box-card card panel-container" :body-style="{ padding: '0px'}">
            <div class="topbar clearfix" align="left">
                <el-button plain icon="el-icon-delete" size="mini">删除</el-button>
                <label class="topbar-item">分类：</label>
                <el-select
                    v-model="classifySelect"
                    placeholder="请选择类别"
                    size="mini"
                    class="classify-select"
                >
                    <el-option
                        v-for="item in options"
                        :key="item.value"
                        :label="item.label"
                        :value="item.value"
                    ></el-option>
                </el-select>
                <label class="topbar-item">日期：</label>
                <el-date-picker
                    class="select-time"
                    v-model="selectTime"
                    type="daterange"
                    size="mini"
                    unlink-panels
                    range-separator="至"
                    start-placeholder="开始日期"
                    end-placeholder="结束日期"
                    :picker-options="pickerOptions"
                ></el-date-picker>
                <label class="topbar-item">搜索：</label>
                <el-input
                    placeholder="请输入关键字"
                    size="mini"
                    v-model="searchText"
                    class="topbar-search"
                >
                    <el-button slot="append" icon="el-icon-search"></el-button>
                </el-input>
                <el-button
                    type="primary"
                    icon="el-icon-plus"
                    size="mini"
                    class="float-right"
                    @click="toPage('/addArticle');"
                >发布</el-button>
            </div>
            <el-table :data="tableData" size="small" style="width: 100%;">
                <el-table-column type="selection" align="center" width="42"></el-table-column>
                <el-table-column align="center" prop="imgUrl" width="90">
                    <template slot="header" slot-scope="scope">封面图片</template>
                    <template slot-scope="scope">
                        <el-image
                            style="margin-top: 4px;width: 60px; height: 60px"
                            :src="scope.row.imgUrl"
                            fit="cover"
                        ></el-image>
                    </template>
                </el-table-column>
                <el-table-column
                    prop="articleName"
                    :show-overflow-tooltip="true"
                    label="文章名称"
                    align="center"
                    sortable
                    width="180"
                ></el-table-column>
                <el-table-column prop="classify" align="center" sortable>
                    <template slot="header" slot-scope="scope">分类</template>
                    <template slot-scope="scope">
                        <el-tag
                            type="info"
                            class="tag-icon"
                            size="mini"
                            v-for="item in scope.row.classify"
                        >{{ item }}</el-tag>
                    </template>
                </el-table-column>
                <el-table-column prop="time" align="center" label="发布时间" sortable></el-table-column>
                <el-table-column prop="author" align="center" label="作者" sortable></el-table-column>
                <el-table-column prop="zan" align="center" label="喜欢" sortable></el-table-column>
                <el-table-column prop="commit" align="center" label="评论" sortable></el-table-column>
                <el-table-column align="center" width="120px" prop="aid">
                    <template slot="header" slot-scope="scope">操作</template>
                    <template slot-scope="scope">
                        <i
                            class="el-icon-edit icon-btn"
                            @click="handleEdit(scope.$index, scope.row.aid)"
                        ></i>
                        <i class="el-icon-delete icon-btn" @click="handleDelete(scope.row.aid)"></i>
                    </template>
                </el-table-column>
            </el-table>
            <!-- 分页 -->
            <el-pagination background class="page-wrap" layout="prev, pager, next" :total="1000"></el-pagination>
        </el-card>
    </div>
</template>

<script>
export default {
    data() {
        return {
            //面包屑路径
            pathList: [
                {
                    path: "/blogListPanel",
                    name: "文章列表"
                }
            ],
            classifySelect: "全部",
            searchText: "", //搜索内容
            options: [
                {
                    value: "选项1",
                    label: "JavaScript"
                },
                {
                    value: "选项2",
                    label: "前端"
                }
            ],
            pickerOptions: {
                //时间选择
                shortcuts: [
                    {
                        text: "最近一周",
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(
                                start.getTime() - 3600 * 1000 * 24 * 7
                            );
                            picker.$emit("pick", [start, end]);
                        }
                    },
                    {
                        text: "最近一个月",
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(
                                start.getTime() - 3600 * 1000 * 24 * 30
                            );
                            picker.$emit("pick", [start, end]);
                        }
                    },
                    {
                        text: "最近三个月",
                        onClick(picker) {
                            const end = new Date();
                            const start = new Date();
                            start.setTime(
                                start.getTime() - 3600 * 1000 * 24 * 90
                            );
                            picker.$emit("pick", [start, end]);
                        }
                    }
                ]
            },
            selectTime: "", //时间选择
            tableData: [
                {
                    aid: "1234",
                    imgUrl: "",
                    articleName: "理解JavaScript执行上下文",
                    classify: ["JavaScript", "前端"],
                    time: "2019-11-05",
                    author: "叫我院长",
                    zan: "520",
                    commit: "999"
                }
            ]
        };
    },
    mounted() {},
    methods: {
        handleEdit(index, aid) {
            this.toPage({
                name: "addArticle",
                params: { aid }
            });
        },
        //删除文章
        handleDelete(aid) {
            this.$confirm("确认要删除此篇文章吗？", "警告！", {
                distinguishCancelAndClose: true,
                confirmButtonText: "确认删除",
                cancelButtonText: "取消"
            })
                .then(() => {
                    this.$message({
                        type: "success",
                        message: "删除成功！"
                    });
                })
                .catch(action => {
                    this.$message({
                        type: "info",
                        message: "已取消删除操作！"
                    });
                });
        },
        toPage(path) {
            this.$router.push(path);
        }
    }
};
</script>

<style scoped>
</style>