<template>
    <el-form :inline="true" :model="formInline">

        <el-form-item label="Message">
            <el-select v-model="formInline.message" clearable placeholder="select message"
                       v-on:visible-change="selectDemo">
                <el-option 
                        v-for="(item, index) in type_options"
                        :key = "index"
                        :label="item.message"
                        :value="item.id">
                </el-option>
            </el-select>
        </el-form-item>

        <el-form-item v-if='formInline.message' label="Description">
            <el-input v-model="formInline.message" placeholder="Please input message"></el-input>
        </el-form-item>

        <el-form-item v-else='formInline.message' label="Description">
            <el-input v-model="formInline.message" disabled placeholder="Please input suffix of message"></el-input>
        </el-form-item>

        <el-form-item>
            <el-button @click="addItem" type="primary" size="large">Add</el-button>
        </el-form-item>
	<db-modal :dialogFormVisible="dialogFormVisible" :form="form" v-on:canclemodal="dialogVisible"></db-modal>
  
    </el-form>
</template>

<script>
    import lodash from 'lodash'
    import Bus from '../eventBus'
    import DbModal from './DbModal.vue'

    export default {
        name: 'db-filterinput',
        data() {
            return {
                dialogFormVisible: false,
                form: '',
                type_options: [],
                formInline: {
                    message: '',
                },
                formLabelWidth: '120px'
            }
        },

        watch: {
            'formInline.message': 'filterResultData',
        },

        components: {
            DbModal
        },

        methods: {
            dialogVisible: function () {
                this.dialogFormVisible = false;
            },

            addItem: function() {
                this.dialogFormVisible = true;
 		this.form = {};
            },

            selectDemo: function (params) {
                if (params) {
                    this.$axios.get("http://10.99.70.98:8080/reminders")
                        .then((response) => {
                            this.type_options = response.data;
                            console.log(response.data);
                        }).catch(function (response) {
                        console.log(response)
                    });
                }

            },
            filterResultData: _.debounce(
                function () {
		    const selectUrl = "http://10.99.70.98:8080/reminders/" + this.formInline.message;
                    this.$axios.get(selectUrl)
			.then((response) => {
                        Bus.$emit('filterResultData1', response.data);
                        console.log(response.data);
                    }).catch(function (response) {
                        console.log(response)
                    });

                },
                500
            ),
        }
    }


</script>

