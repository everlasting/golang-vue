<template>
    <div>
        <el-table
                :data="tableData"
                border
                style="width: 100%"
                class="table">
            <el-table-column
                    fixed
                    prop="id"
                    label="item_id"
                    width="100">
            </el-table-column>
            <el-table-column
                    prop="message"
                    label="message"
                    width="120">
            </el-table-column>
            <el-table-column
                    prop="createdAt"
                    label="createdAt"
                    width="220">
            </el-table-column>
            <el-table-column
                    prop="updatedAt"
                    label="updatedAt"
                    width="220">
            </el-table-column>
            <el-table-column
                    fixed="right"
                    label="Operation"
                    width="150">
                <template scope="scope">
                    <el-button @click="editItem(scope.$index, tableData)" type="text" size="large">Edit</el-button>
                    <el-button @click="deleteItem(scope.$index, tableData)" type="text" size="large">Delete</el-button>
                </template>
            </el-table-column>
        </el-table>
        <db-modal :dialogFormVisible="dialogFormVisible" :form="form" v-on:canclemodal="dialogVisible"></db-modal>
    </div>

</template>

<script>
    import Bus from '../eventBus'
    import DbModal from './DbModal.vue'

    export default {
        data(){
            return {
                tableData: [],
                apiUrl: 'http://10.99.70.98:8080/reminders',
                dialogFormVisible: false,
                form: '',
            }
        },
        components: {
            DbModal
        },
        mounted () {
            this.getCustomers();
            Bus.$on('filterResultData1', (data) => {
                this.tableData = [data];
            });
        },

        methods: {

            dialogVisible: function () {
                this.dialogFormVisible = false;
            },

            getCustomers: function () {
                this.$axios.get(this.apiUrl)
                .then((response) => {
                    console.log(response);
                    this.tableData = response.data;
                }).catch(function (response) {
                    console.log('getCustomers error')
                    console.log(response)
                });
            },
            editItem: function (index, rows) {
                this.dialogFormVisible = true;
		this.form = {};
                const itemId = rows[index].id;
                const idurl = 'http://10.99.70.98:8080/reminders/' + itemId;
                this.$axios.get(idurl).then((response) => {
                    this.form = response.data;
                }).catch(function (response) {
 		    console.log('editItem error') 
                    console.log(response)
                });
            },

            deleteItem: function (index, rows) {
                const itemId = rows[index].id;
                const idurl = 'http://10.99.70.98:8080/reminders/' + itemId;
                this.$axios.delete(idurl).then((response) => {
                    this.getCustomers();
                }).catch(function (response) {
                    console.log(response)
                });
//		location.reload();
            },


            formatter(row, column) {
                let data = this.$moment(row.createdAt, this.$moment.ISO_8601);
                return data.format('YYYY-MM-DD')
            },
        }
    }
</script>

<style>
    .table {
        margin-top: 30px;
    }

    .pagination {
        margin-top: 10px;
        float: right;
    }

</style>
