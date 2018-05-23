<template>
  <div class="home">
    <nav class="navbar navbar-default navbar-fixed-top" style="background-color:white">
          <div class="container">
              <div class="navbar-header">
                  <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                  <span class="sr-only">Toggle navigation</span>
                      <span class="icon-bar"></span>
                      <span class="icon-bar"></span>
                      <span class="icon-bar"></span>
                  </button>
                  <a class="navbar-brand" href="#">KANBAN STICKY NOTE</a>
              </div>

              <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
                  <ul class="nav navbar-nav navbar-right">
                      <li style="background-color:#d1ecf1"><a data-toggle="modal" data-target="#addtask" style="cursor:pointer"><span class="glyphicon glyphicon-plus"> </span> Add new task</a></li>
                  </ul>
              </div>
          </div>
    </nav>

    <div class="container" style="margin-top:50px; padding:0;">

        <div class="largebox">
          <div class="col-md-3">
            <h3>Backlog</h3>
              <div class="thumbnail">

                <Smallbox 
                v-for="box in backlog"
                :smallbox="box"
                :namestatus="'backlog'"
                :nextbutton="next"
                :backbutton="back"
                :key="box['.key']"
                @button-save="buttonSave"
                @detail-button="buttonDetail"
                @button-next="buttonNext"
                @button-back="buttonBack"
                @button-delete="buttonDelete"
                />

            </div>
          </div>
        </div>

        <div class="largebox">
          <div class="col-md-3">
            <h3>Upcoming</h3>
              <div class="thumbnail">

                <Smallbox 
                v-for="box in upcoming"
                :smallbox="box"
                :namestatus="'upcoming'"
                :nextbutton="next"
                :backbutton="back"
                :key="box['.key']"
                @button-save="buttonSave"
                @detail-button="buttonDetail"
                @button-next="buttonNext"
                @button-back="buttonBack"
                @button-delete="buttonDelete"
                />

            </div>
          </div>
        </div>

        <div class="largebox">
          <div class="col-md-3">
            <h3>In Progress</h3>
              <div class="thumbnail">

                <Smallbox 
                v-for="box in inProgress"
                :smallbox="box"
                :namestatus="'inprogress'"
                :nextbutton="next"
                :backbutton="back"
                :key="box['.key']"
                @button-save="buttonSave"
                @detail-button="buttonDetail"
                @button-next="buttonNext"
                @button-back="buttonBack"
                @button-delete="buttonDelete"
                />

            </div>
          </div>
        </div>

        <div class="largebox">
          <div class="col-md-3">
            <h3>Complete</h3>
              <div class="thumbnail">

                <Smallbox 
                v-for="box in complete"
                :smallbox="box"
                :namestatus="'complete'"
                :nextbutton="next"
                :backbutton="back"
                :key="box['.key']"
                @button-save="buttonSave"
                @detail-button="buttonDetail"
                @button-next="buttonNext"
                @button-back="buttonBack"
                @button-delete="buttonDelete"
                />

            </div>
          </div>
        </div>

    </div>

    <div id="addtask" class="modal fade" role="dialog">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal">&times;</button>
                    <h4 class="modal-title">Add Task</h4>
                </div>
                <div class="modal-body">
                    <div class="form-group">
                    <label>Add Task</label>
                        <input type="text" class="form-control" v-model="newKanban.addtask" maxlength="30" />
                        <i style="color:red" v-if="newKanban.addtask.length >= 1 && newKanban.addtask.length < 5">Maximum length char 30</i>
                    </div>
                    <div class="form-group">
                    <label>Description</label>
                        <textarea type="text" class="form-control" v-model="newKanban.description" maxlength="200"></textarea>
                        <i style="color:red" v-if="newKanban.description.length >= 1 && newKanban.description.length < 20">Maximum length char 200</i>
                    </div>
                    <div class="form-group">
                    <label>Position</label>
                        <select v-model="status" class="form-control">
                          <option value=1>Backlog</option>
                          <option value=2>Up Coming</option>
                          <option value=3>In Progress</option>
                          <option value=4>Complete</option>
                        </select>
                    </div>
                    <button @click="addTask" type="submit" class="btn" data-dismiss="modal" style="background-color:#d1ecf1">Add Task</button>
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
import firebase from 'firebase/app'
import 'firebase/database'
import swal from 'sweetalert'

import Smallbox from '@/components/Smallbox.vue'

let app = firebase.initializeApp({
  databaseURL: 'https://kanban-61641.firebaseio.com/',
  projectId: 'kanban-61641',
})

let db = app.database()
let kanban = db.ref('kanbanlist')

export default {
  name: 'home',
  firebase: {
    box: kanban
  },
  components: {
    Smallbox,
  },
  data: function () {
    return {
      status: '',
      newKanban: 
      {
        addtask: '',
        description: ''
      },
      back: {
        button: true,
        name: ''
      },
      next: {
        button: true,
        name: ''
      }
    }
  },
  computed: {
    backlog () {
      return this.box.filter((boxlist) => boxlist.status === 1)
    },
    upcoming () {
      return this.box.filter((boxlist) => boxlist.status === 2)
    },
    inProgress () {
      return this.box.filter((boxlist) => boxlist.status === 3)
    },
    complete () {
      return this.box.filter((boxlist) => boxlist.status === 4)
    }
  },
  methods: {
    addTask: function () {
      kanban.push({addtask:this.newKanban.addtask, description:this.newKanban.description, status:Number(this.status)})
      this.newKanban.addtask = ''
      this.newKanban.description = ''
      swal('Success!', `Success add task ${this.newKanban.addtask}`, 'success')
    },
    buttonDetail: function (name) {
      if (name == 'backlog') {

        this.next.name = 'Upcoming'
        this.back.button = false
        this.next.button = true
        
      } else if (name == 'upcoming') {

        this.back.name = 'Backlog'
        this.next.name = 'In Progress'
        this.back.button = true
        this.next.button = true

      } else if (name == 'inprogress') {

        this.back.name = 'Upcoming'
        this.next.name = 'Complete'
        this.back.button = true
        this.next.button = true

      } else {

        this.back.placeholder = 'In Progress'
        this.back.button = true
        this.next.button = false

      }
    },
    buttonNext: function (key,status) {
      kanban.child(key).child('status').set(status + 1)
    },
    buttonBack: function (key,status) {
      kanban.child(key).child('status').set(status - 1)
    },
    buttonDelete: function (name, key) {
      swal({
        title: 'Are you sure?',
        icon: 'warning',
        buttons: true,
        dangerMode: true,
      })
      .then((willDelete) => {
        if (willDelete) {
          swal(`success delete task ${name}`, {
            icon: 'success',
          })
          kanban.child(key).remove()
        }else{
          swal(`Failed delete task ${name}`)
        }
      })
    },
    buttonSave: function (key, addtask, description, status) {
      kanban.child(key).update({ addtask, description, status: Number(status)})
      swal('Success!', `Success save task ${addtask}`, 'success')
    }
  }
}
</script>

<style lang="scss" scoped>
    body{
      background-color: #fafafa;
    }
    .dropdown-menu{
        min-width: 200px;
    }
</style>
