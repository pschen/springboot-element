<template>
	<el-dialog ref="dialog" title="节点列表">
		<el-row class="pos-rel h-60">
			<el-input placeholder="请输入内容" v-model="keyword" class="search-btn w-300">
				<el-button slot="append" icon="search" @click="searchMsg()"></el-button>
			</el-input>
		</el-row>
		<el-row>
			<el-table :data="tableData" stripe style="width: 100%">
				<el-table-column 	prop="type" label="类型" wiwdth="100"> </el-table-column>
				<el-table-column prop="title" label="规则名称"> </el-table-column>
				<el-table-column prop="name" label="规则标识" width="180"> </el-table-column>
				<el-table-column inline-template label="操作" width="100">
					<el-row>
						<el-button size="small" @click="selectRule(row)">选择</el-button>
					</el-row>
				</el-table-column>
			</el-table>
		</el-row>
	</el-dialog>
</template>
<style>
.search-btn {
	position: absolute;
	top: 0px;
	right: 0px;
}
</style>
<script>
  import http from '../../../../assets/js/http'

  export default {
    data() {
      return {
        keyword: '',
        tableData: []
      }
    },
    methods: {
      open() {
        this.$refs.dialog.open()
      },
      closeDialog() {
        this.$refs.dialog.close()
      },
      selectRule(item) {
        setTimeout(() => {
          this.$parent.form.ruleName = item.title;
          this.$parent.form.ruleId = item.id
        }, 0);
        this.closeDialog()
      },
      getRules() {
        this.$http.apiPost('admin/rules').then((res) => {
          this.$http.handelResponse(res, (data) => {
            this.tableDataShow = _(data.list).forEach((ret) => {
              ret.showInput = false
            });
            this.tableData = this.tableDataShow
          })
        })
      }
    },
    created() {
      let data = store.state.rules;
      if (data && data.length) {
        this.tableDataShow = _(data.list).forEach((res) => {
          res.showInput = false
        });
        this.tableData = this.tableDataShow
      } else {
        this.getRules()
      }
    },
    mixins: [http]
  }
</script>
