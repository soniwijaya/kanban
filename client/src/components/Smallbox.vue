<template>
    <div class="smallbox">
        <div class="caption jumbotron padding color-yellow marginb" style="text-align:left">
            <div class="input-group">
                <h4><b>{{ smallbox.addtask }}</b></h4>
                <h5>{{ smallbox.description }}</h5>
                <span class="input-group-btn">
                    <a @click="detail" data-toggle="modal" :data-target="'#'+smallbox['.key']" style="cursor:pointer">
                        <i class="glyphicon glyphicon-chevron-down dropdown-toggle" style="font-size:20px"></i>
                    </a>
                </span>
            </div>
        </div>

        <div :id="smallbox['.key']" class="modal fade" role="dialog">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <button type="button" class="close" data-dismiss="modal">&times;</button>
                        <h4 class="modal-title">Detail task</h4>
                    </div>
                    <div class="modal-body">
                        <div class="form-group jumbotron padding color-green marginb">
                            <h4>Task todo</h4>
                            <input class="form-control" type="text" v-model="nameTask" />
                        </div>
                        <div class="form-group jumbotron padding color-blue marginb">
                            <h4>Description task</h4>
                            <textarea rows="3" class="form-control" type="text" v-model="descriptionTask" ></textarea>
                        </div>
                        <div class="form-group jumbotron padding color-yellow marginb">
                            <h4>Task position</h4>
                            <select v-model="statusTask" class="form-control">
                                <option v-if="smallbox.status == 1" selected value=1>Backlog</option>
                                <option v-if="smallbox.status == 2" selected value=2>Up Coming</option>
                                <option v-if="smallbox.status == 3" selected value=3>In Progress</option>
                                <option v-if="smallbox.status == 4" selected value=4>Complete</option>
                                <option value=1>Backlog</option>
                                <option value=2>Up Coming</option>
                                <option value=3>In Progress</option>
                                <option value=4>Complete</option>
                            </select>
                        </div>
                        <button @click="buttonBack" v-show="backbutton.button" type="button" class="btn btn-warning" data-dismiss="modal"><span class="glyphicon glyphicon-chevron-left"> </span> {{ backbutton.name }}</button>
                        <button @click="saveButton" type="button" class="btn btn-success" data-dismiss="modal"><span class="glyphicon glyphicon-floppy-open"> </span> Save</button>
                        <button @click="deleteButton" type="button" class="btn btn-danger" data-dismiss="modal"><span class="glyphicon glyphicon-trash"> </span> Delete</button>
                        <button @click="buttonNext" v-show="nextbutton.button" type="button" class="btn btn-primary" data-dismiss="modal">{{ nextbutton.name }} <span class="glyphicon glyphicon-chevron-right"></span></button>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
    
</template>

<script>
export default {
    name: 'smallbox',
    props: ['smallbox','namestatus','nextbutton','backbutton'],
    data: function () {
        return {
            nameTask: this.smallbox.addtask,
            descriptionTask: this.smallbox.description,
            statusTask: this.smallbox.status,
            key: this.smallbox['.key'],
            status: this.smallbox.status,
            name: this.namestatus
        }
    },
    methods: {
        detail: function () {
            this.$emit('detail-button', this.name)
        },
        buttonNext: function () {
            this.$emit('button-next', this.key, this.status)
        },
        buttonBack: function () {
            this.$emit('button-back', this.key, this.status)
        },
        deleteButton: function () {
            this.$emit('button-delete',this.nameTask, this.key)
        },
        saveButton: function () {
            this.$emit('button-save', this.key,this.nameTask, this.descriptionTask, this.statusTask)
        }
    }
}
</script>

<style scoped>
    .padding{
        padding-top: 15px;
        padding-bottom: 15px;
    }
    .color-green{
        background-color: #d4edda
    }
    .color-blue{
        background-color: #d1ecf1
    }
    .color-yellow{
        background-color: #fff3cd
    }
    .marginb{
        margin-bottom: 5px;
    }
</style>
