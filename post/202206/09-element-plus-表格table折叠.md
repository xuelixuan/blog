# element-plus-表格 table 折叠与展开

table 添加折叠功能

```html
<el-table
	ref="taskList"
	@expand-change="expandChange"
	:data="list"
	empty-text="暂无数据"
	style="width: 100%"
>
	<el-table-column type="expand" width="30">
		<template #default="scope">
			<div class="expand">
				<div v-if="!showColumn" class="expand-info">
					<div class="info-item">
						<div>浏览</div>
						<div>
							{{scope.row.watchProcess}}/{{scope.row.watchCount}}
						</div>
					</div>
					<div class="info-item">
						<div>点赞</div>
						<div>
							{{scope.row.diggProcess}}/{{scope.row.diggCount}}
						</div>
					</div>
					<div class="info-item">
						<div>评论</div>
						<div>
							{{scope.row.commentProcess}}/{{scope.row.commentCount}}
						</div>
					</div>
					<div class="info-item">
						<div>任务周期</div>
						<div>
							{{scope.row.startTime}} 至 {{scope.row.endTime}}
						</div>
					</div>
				</div>
				<!-- :header-cell-style="{backgroundColor: 'rgb(230 231 232) !important'}" -->
				<el-table
					class="expand-table"
					header-row-class-name="expand-table-head"
					:data="scope.row.commentList"
					max-height="322"
					style="width: 100%"
				>
					<el-table-column prop="id" label="ID" width="80" />
					<el-table-column
						width="180"
						prop="CompleteDate"
						label="预期执行时间"
					/>
					<el-table-column prop="rContent" label="评论内容" />
					<el-table-column width="120" prop="status" label="是否完成">
						<template #default="scope">
							<span
								class="task-status"
								v-if="scope.row.status == '5'"
							>
								<i class="dot dot-2"></i>
								<span>已完成</span>
							</span>
							<span class="task-status" v-else>
								<i class="dot dot-1"></i>
								<span>待完成</span>
							</span>
						</template>
					</el-table-column>
					<el-table-column
						width="100"
						prop="diggCount"
						label="点赞数"
					/>
					<el-table-column
						width="100"
						prop="watchCount"
						label="浏览数"
					/>
				</el-table>
			</div>
		</template>
	</el-table-column>
</el-table>
```

默认可以实现多个展开状态同时存在,如果只允许有一个展开项:

```js
// table折叠与隐藏
expandChange(row, expand) {
    if (expand.length > 0) {
        expand.forEach(expandItem => {
            if (expandItem.id === row.id) {
                // 展开当前行
                this.$refs.taskList.toggleRowExpansion(expandItem, true)
            } else {
                // 隐藏其他行
                this.$refs.taskList.toggleRowExpansion(expandItem, false)
            }
        });
    }
},
```
