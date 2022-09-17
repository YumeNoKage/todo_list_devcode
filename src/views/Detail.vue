<style scoped>
    .border-top-12,
    .border-top-12:hover {
        border-radius: 12px 12px 0px 0px;
    }
    .border-bottom-12,
    .border-bottom-12:hover {
        border-radius: 0px 0px 12px 12px;
    }

    .input-dropdown:not(.show) .chevron img{
        transform: rotate(180deg) !important;
    }
    .input-dropdown.show .chevron img{
        transform: rotate(0deg) !important;
    }

</style>
<template>
    <div class="row container mx-auto py-5">
        <!-- loading animation -->
        <div class="col-12 mt-5 pt-3" v-if="isLoading && data == null" data-cy="activity-detail-loading">
            <Loading />
        </div>

        <!-- header -->
        <div class="col-12" v-if="data != null" data-cy="activity-item">
            <div class="d-flex justify-content-between">
                <div data-cy="activity-title" class="font-custome-36 fw-custome-700">
                    <a href="/" class="border-0 text-decoration-none text-custome-black" data-cy="todo-back-button">
                        <img src="../assets/icons/back-left.svg" alt="back">
                    </a>
                    <input v-if="editTitle" type="text" id="editTitleTodo" v-model="data.title" :class="`border-0 ${editTitle ? 'border-bottom' : ''} fw-custome-700`" @change="updateTitle(), editTitle = false" @click="editTitle = true" @blur="editTitle = false" style="outline: none;" data-cy="todo-title" :readonly="!editTitle" focusable>
                    <label v-if="!editTitle" @click="editTitle = !editTitle" for="editTitleTodo" :data-cy="!editTitle ? 'todo-title' : ''"><h1 class="font-custome-36 fw-custome-700">{{data.title}}</h1></label>
                    <span class="ms-3">
                        <button class="bg-transparent border-0" @click="editTitle = !editTitle" data-cy="todo-title-edit-button">
                            <img width="24" src="../assets/icons/pencil.svg" alt="edit">
                            <!-- <img width="24" src="../assets/icons/pencil.svg" alt="edit" v-if="!editTitle"> -->
                            <!-- <img width="24" src="../assets/icons/close.svg" alt="edit" v-if="editTitle"> -->
                        </button>
                    </span>
                </div>
                
                <div class="d-flex">
                    <div class="dropdown me-4">
                        <div class="d-flex position-relative col-lg-6 col-md-8 col-sm-8 input-dropdown" data-bs-toggle="dropdown" aria-expanded="false" data-cy="todo-sort-button">
                            <span class="border rounded-circle">
                                <img class="p-3" src="../assets/icons/arrows-sort.svg" alt="sort">
                            </span>
                        </div>
                        <ul class="dropdown-menu py-0 rounded-12" style="min-width: 205px">
                            <li data-cy="sort-selection">
                                <div :class="`dropdown-item d-flex align-items-center justify-content-between py-3 border-bottom border-top-12`" @click="filter= 'latest'" :data-cy="filter == 'latest' ? 'sort-selection-selected' : false">
                                    <div class="d-flex align-items-center">
                                        <span data-cy="sort-selection-icon"><img src="../assets/icons/arrow-sort-desc.svg" alt="sort desc"></span>
                                        <span class="ms-3" data-cy="sort-selection-title">Terbaru</span>
                                    </div>
                                    <div v-if="filter == 'latest'">
                                        <img src="../assets/icons/check.svg" alt="checked">
                                    </div>
                                </div>
                            </li>
                            <li data-cy="sort-selection">
                                <div :class="`dropdown-item d-flex align-items-center justify-content-between py-3 border-bottom`" @click="filter= 'oldest'" :data-cy="filter == 'oldest' ? 'sort-selection-selected' : false">
                                    <div class="d-flex align-items-center">
                                        <span data-cy="sort-selection-icon"><img src="../assets/icons/arrow-sort-asc.svg" alt="sort asc"></span>
                                        <span class="ms-3" data-cy="sort-selection-title">Terlama</span>
                                    </div>
                                    <div v-if="filter == 'oldest'">
                                        <img src="../assets/icons/check.svg" alt="checked">
                                    </div>
                                </div>
                            </li>
                            <li data-cy="sort-selection">
                                <div :class="`dropdown-item d-flex align-items-center justify-content-between py-3 border-bottom`" @click="filter= 'az'" :data-cy="filter == 'az' ? 'sort-selection-selected' : false">
                                    <div class="d-flex align-items-center">
                                        <span data-cy="sort-selection-icon"><img src="../assets/icons/arrow-sort-az.svg" alt="sort az"></span>
                                        <span class="ms-3" data-cy="sort-selection-title">A-Z</span>
                                    </div>
                                    <div v-if="filter == 'az'">
                                        <img src="../assets/icons/check.svg" alt="checked">
                                    </div>
                                </div>
                            </li>
                            <li data-cy="sort-selection">
                                <div :class="`dropdown-item d-flex align-items-center justify-content-between py-3 border-bottom`" @click="filter= 'za'" :data-cy="filter == 'za' ? 'sort-selection-selected' : false">
                                    <div class="d-flex align-items-center">
                                        <span data-cy="sort-selection-icon"><img src="../assets/icons/arrow-sort-za.svg" alt="sort za"></span>
                                        <span class="ms-3" data-cy="sort-selection-title">Z-A</span>
                                    </div>
                                    <div v-if="filter == 'za'">
                                        <img src="../assets/icons/check.svg" alt="checked">
                                    </div>
                                </div>
                            </li>
                            <li data-cy="sort-selection">
                                <div :class="`dropdown-item d-flex align-items-center justify-content-between py-3 border-bottom-12`" @click="filter= 'unfinished'" :data-cy="filter == 'unfinished' ? 'sort-selection-selected' : false">
                                    <div class="d-flex align-items-center">
                                        <span data-cy="sort-selection-icon"><img src="../assets/icons/arrows-sort-unfinished.svg" alt="sort unfinished"></span>
                                        <span class="ms-3" data-cy="sort-selection-title">Belum Selesai</span>
                                    </div>
                                    <div v-if="filter == 'unfinished'">
                                        <img src="../assets/icons/check.svg" alt="checked">
                                    </div>
                                </div>
                            </li>
                        </ul>
                    </div>

                    <button class="btn bg-custome-blue-shoft rounded-pill px-3 text-white font-custome-18 fw-custome-600" data-bs-target="#addTodo" data-bs-toggle="modal" id="modalAddTodo" data-cy="todo-add-button">
                        <img src="../assets/icons/plus.svg" alt="add" width="24">
                        <span class="ms-2">Tambah</span>
                    </button>
                </div>
            </div>
        </div>

        <!-- list activity -->
        <div class="col-12" v-if="data != null">
            <Detail :showLoading="isLoading" ref="ListTodo" :dataParent="todo_items" @editData="matchFormTodo($event)" @update:Parent="updateParent()"/>
        </div>

        <!-- modal add todo -->
        <div class="modal fade" id="addTodo" tabindex="-1" aria-labelledby="addTodoLabel" aria-hidden="true" data-cy="modal-add">
            <div class="modal-dialog modal-dialog-centered modal-fullscreen-md-down">
                <div class="modal-content rounded-12 border-0">
                    <div class="modal-header">
                        <h5 class="modal-title font-18 fw-custome-600 text-custome-black" data-cy="modal-add-title">Tambah List Item</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" data-cy="modal-add-close-button"></button>
                    </div>
                    <div class="modal-body px-0">
                        <form @submit.prevent="todo.id && (todo.title != null || todo.title != '') ? updateTodo() : addItem()" data-cy="modal-add-save-button">
                            <div class="mb-3 modal-body py-0">
                                <label class="form-label font-custome-12 fw-custome-600 text-custome-black" data-cy="modal-add-name-title">NAMA LIST ITEM</label>
                                <input v-model="todo.title" type="text" class="form-control font-custome-16 fw-custome-400 text-custome-black px-3 py-3" id="inputNameItem" placeholder="Tambahkan nama list item" data-cy="modal-add-name-input" required>
                            </div>
                            <div class="mb-3 modal-body py-0">
                                <label for="property" class="form-label font-custome-12 fw-custome-600 text-custome-black" data-cy="modal-add-priority-title">PRIORITY</label>
                                <div class="dropdown">
                                    <div class="d-flex position-relative col-lg-6 col-md-8 col-sm-8 input-dropdown" data-bs-toggle="dropdown" aria-expanded="false" data-cy="modal-add-priority-dropdown">
                                        <div style="width: 14px; height: 14px; transform: translatey(-50%)" :class="`rounded-circle bg-custome-${todo.color} position-absolute top-50 ms-3`"></div>
                                        <input type="text" class="form-control font-16 fw-custome-400 text-custome-black ps-5 pe-3 py-3 dropdown-toggle" :value="todo.priority_name" readonly placeholder="Pilih priority" id="property" required>
                                        <span style="transform: translatey(-50%)" class="position-absolute top-50 end-0 me-3 chevron"><img src="../assets/icons/chevron-down.svg" alt="chevron"></span>
                                    </div>
                                    <ul class="dropdown-menu py-0 rounded-12" style="min-width: 205px">
                                        <!-- <li>
                                            <div class="dropdown-item d-flex align-items-center justify-content-between py-3 border-bottom border-top-12" style="background-color: #E5E5E5;" >
                                                <div class="d-flex align-items-center">
                                                    <span class="ms-3">Pilih priority</span>
                                                </div>
                                            </div>
                                        </li> -->
                                        <li v-for="(item, i) in getPriority(null, 'option')" :key="i">
                                            <div :class="`dropdown-item d-flex align-items-center justify-content-between py-3 border-bottom ${i == getPriority(null, 'option').length - 1 ? 'border-bottom-12' : ''} ${i == 0 ? 'border-top-12' : ''}`" @click="todo.priority = item.value, todo.priority_name = item.name, todo.color = item.color" data-cy="modal-add-priority-item">
                                                <div class="d-flex align-items-center">
                                                    <div style="width: 14px; height: 14px;" :class="`rounded-circle bg-custome-${item.color}`"></div>
                                                    <span class="ms-3">{{item.name}}</span>
                                                </div>
                                                <div v-if="todo.priority_name == item.name">
                                                    <img src="../assets/icons/check.svg" alt="checked">
                                                </div>
                                            </div>
                                        </li>
                                    </ul>
                                </div>
                            </div>
                            <div class="modal-footer">
                                <button class="btn bg-custome-blue-shoft rounded-pill px-3 text-white font-custome-18 fw-custome-600" :type="todo.title == null || todo.title == '' ? 'button':'submit'" :disabled="todo.title == null || todo.title == '' ? true : false">Simpan</button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>

        <!-- modal information -->
        <div class="modal fade" id="updateTitleNotif" tabindex="-1" aria-labelledby="updateTitleNotifLabel" aria-hidden="true" data-cy="modal-information">
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content rounded-12 border-0">
                    <div class="modal-body border-0">
                        <div class="desc text-left">
                            <span class="font-14 fw-custome-500 text-custome-black">
                                <img src="../assets/icons/info.svg" class="to-red me-2">
                                {{message}}
                            </span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

</template>

<script>
    import { mapActions } from 'vuex';
    import Detail from "../components/todo_list/Detail.vue"

    export default {
        components:{Detail},
        data(){
            return{
                isLoading: false,
                data: null,
                editTitle: false,
                todo_items: null,
                todo:{
                    activity_group_id: this.$route.params.id,
                    priority: 'very-high',
                    priority_name: 'Very High',
                    color: 'red',
                    title: null,
                },
                filter: 'latest',
                dataReversed: false,
                message: null,
            }
        },

        watch:{
            'filter': function(newValue, oldValue){
                const realData = this.data.todo_items
                let newData

                if (!this.dataReversed && (newValue == 'oldest' || newValue == 'za' || newValue == 'unfinished')) {
                    newData = realData.reverse()
                    this.dataReversed = true
                } else if (!this.dataReversed && (newValue == 'latest' || newValue == 'az')){
                    newData = realData
                    this.dataReversed = false
                } else if (this.dataReversed && (newValue == 'unfinished' || newValue == 'oldest' || newValue == 'za')){
                    newData = realData
                    this.dataReversed = true
                } else if (this.dataReversed && (newValue == 'latest' || newValue == 'az')){
                    newData = realData.reverse()
                    this.dataReversed = false
                }

                this.todo_items = newData
            }
        },

        methods: {
            ...mapActions({
                getData: 'getData',
                postData: 'postData',
                updateData: 'updateData'
            }),

            async getDetail(url, params){
                try {
                    this.isLoading = true
                    let data = {url, params}
                    const response = await this.getData(data);
                    if (response.status == 200) {
                        this.isLoading = false
                        this.data = response.data
                        this.todo_items = this.data.todo_items
                    }
                } catch (error) {
                    console.error(error);
                }
            },

            async updateTitle(){
                try {
                    let data = {
                        url : `activity-groups/${this.$route.params.id}`, 
                        params : {title : this.data.title}
                    }
                    const response = await this.updateData(data);
                    if (response.status == 200) {
                        this.message = 'Activity berhasil diupdate'
                        
                        $('#updateTitleNotif').modal('show');
                        this.getDetail(`activity-groups/${this.$route.params.id}`,null)
                        
                        setTimeout(() =>{ $('#updateTitleNotif').modal('hide'), this.message = null},1000)
                        this.editTitle = false
                    }
                } catch (error) {
                    console.error(error);
                }
            },
            
            async addItem(){
                try {
                    this.isLoading = true

                    let data = {
                        url : 'todo-items', 
                        params : {...this.todo}
                    }
                    const response = await this.postData(data);
                    if (response.status == 201) {
                        this.getDetail(`activity-groups/${this.$route.params.id}`, null)
                        
                        $('#addTodo').modal('hide');
                        
                        // this.message = 'Todo berhasil ditambahkan'
                        // $('#updateTitleNotif').modal('show');
                        
                        this.data = null
                        
                        // setTimeout(() =>{$('#updateTitleNotif').modal('hide'); this.message = null},1000)
                        
                        this.todo = {
                            activity_group_id: this.$route.params.id,
                            priority: 'very-high',
                            priority_name: 'Very High',
                            color: 'red',
                            title: null,
                        }
                    }
                } catch (error) {
                    console.error(error);
                }
            },

            async updateTodo(){
                try {
                    this.isLoading = true

                    let data = {
                        url : `todo-items/${this.todo.id}`, 
                        params : { ...this.todo }
                    }
                    const response = await this.updateData(data);
                    if (response.status == 200) {
                        this.getDetail(`activity-groups/${this.$route.params.id}`, null)
                        // this.message = 'Todo berhasil diupdate'

                        
                        // $('#updateTitleNotif').modal('show');
                        
                        this.data = null;
                        // setTimeout(() =>{ $('#updateTitleNotif').modal('hide'), this.message = null},1000)
                        
                        this.todo = {
                            activity_group_id: this.$route.params.id,
                            priority: 'very-high',
                            priority_name: 'Very High',
                            color: 'red',
                            title: null,
                        }
                    }
                    $('#addTodo').modal('hide')
                } catch (error) {
                    console.error(error);
                }
            },

            matchFormTodo(data){
                this.todo.priority = data.priority
                this.todo.priority_name = this.getPriority(data.priority, null).name
                this.todo.color = this.getPriority(data.priority, null).color
                this.todo.title = data.title
                this.todo.is_active = data.is_active
                this.todo.id = data.id

                $('#addTodo').modal('show')
            },

            updateParent(){
                this.data = null
                this.message = null
                this.getDetail(`activity-groups/${this.$route.params.id}`, null)
            },
        },

        mounted(){
            this.getDetail(`activity-groups/${this.$route.params.id}`, null)
        }
    }
        
</script>