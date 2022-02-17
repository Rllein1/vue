<template>
    <div class="container">
        <!-- Modal -->
        <div class="" v-if="showModal" @close="showModal = false">
        <div class="modal-dialog">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" @click="showModal=false"></button>
            </div>
            <div class="modal-body">
                <div class="from-group">
                    <input type="text" class="form-control" v-model="editform.task">
                    <input type="text" class="form-control" style="display:none" v-model="editform.id">
                </div>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-primary" @click="update">Save chaanges</button>
            </div>
            </div>
        </div>
        </div>
        <div class="row justify-content-center">
            <div class="col">
                <div class="card">
                    <table class="table">
                    <thead>
                        <tr>
                        <th scope="col">task</th>
                        <th scope="col">action</th>

                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(task, index) in tasklist" :key="task.id">
                            <td><a href="#" @click="edit(task)">{{task.task}}</a></td>
                            <td><button class="btn" @click="remove(task.id,index)">delete</button></td>
                        </tr>
                    </tbody>
                    <tfoot>
                        <tr>
                            <td>
                                <div class="from-group">
                                    <input type="text" class="form-control" v-model="form.task">
                                </div>
                            </td>
                            <td>
                                <div class="from-group">
                                    <button class="btn" type="submit" @click="submit"> submit</button>
                                </div>
                            </td>
                        </tr>
                    </tfoot>  
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        props:['tasks'],
        data(){
            return{
                tasklist:this.tasks,
                form:{
                    task:null
                },
                editform:{
                    task:null,
                    id:null,
                },
                showModal: false
            }
        },
        methods:{
            submit(){
                const x=this;
                axios.post('/todo',this.form)
                .then(function (response) {
                    x.tasklist.push(response.data);
                    x.form.task=null;
                })
                .catch(function (error) {
                    console.log(error);
                });
            },
            remove(id,index){
            const x=this;
            axios.delete('/todo/'+id)
            .then(function (response) {
                x.tasklist.splice(index,1);
            })
            .catch(function (error) {
                console.log(error);
            });
            },
            edit(task){
                const x=this;
                x.showModal=true;          
                this.editform.task = task.task;
                this.editform.id = task.id;
            },
            update(){
                const x=this;
                axios.put('/todo/'+this.editform.id,this.editform)
                .then(function (response) {
                    location.reload();
                })
                .catch(function (error) {
                    console.log(error);
                });
            }

        }
    }
</script>