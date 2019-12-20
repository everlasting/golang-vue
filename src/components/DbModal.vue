<template>
    <el-dialog title="Edit" v-model="dialogFormVisible" :close-on-click-modal="false" :show-close="true">
        <el-form :model="form">
            <el-form-item label="item_id" :label-width="formLabelWidth">
                <el-input :disabled="true" v-model="form.id" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item label="message" :label-width="formLabelWidth">
                <el-input :disabled="false" v-model="form.message" auto-complete="off"></el-input>
            </el-form-item>

            <el-form-item label="created_at" :label-width="formLabelWidth">
                <el-input :disabled="true" v-model="form.created_at" auto-complete="off"></el-input>
            </el-form-item>

            <el-form-item label="updated_at" :label-width="formLabelWidth">
                <el-input :disabled="true" v-model="form.updated_at" auto-complete="off"></el-input>
            </el-form-item>

        </el-form>
        <div slot="footer" class="dialog-footer">
            <el-button :plain="true" type="danger" v-on:click="canclemodal">Cancel</el-button>
            <el-button :plain="true" @click="updateForm(form)">Save</el-button>
        </div>
    </el-dialog>
</template>


<script>
    export default {
        data(){
            return {
                formLabelWidth: '120px',
            }
        },
        props: ['dialogFormVisible', 'form'],

        methods: {
            updateForm: function (formName) {
                if (typeof(formName.id) == "undefined") {
			let message = formName.message;
	                this.$axios.post('http://10.99.70.98:8080/reminders', {
        	            message: message,
                	})
                    	.then(function (response) {
                        	console.log(response);
                        	this.form = response.data;
                    	})
                    	.catch(function (error) {
                        	console.log(error);
                    	});
                	location.reload();
		} else {
                let itemId = formName.id;
                let message = formName.message;
                let created_at = formName.created_at;
                this.$axios.put('http://10.99.70.98:8080/reminders/' + itemId, {
                    message: message,
                })
                    .then(function (response) {
                        console.log(response);
                        this.form = response.data;
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
                location.reload();
            }
		},

            canclemodal: function () {
                this.$emit('canclemodal');
            }
        }

    }

</script>
