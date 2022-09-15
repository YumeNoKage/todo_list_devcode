<template>
    <div class="row container mx-auto py-5">
        <!-- header -->
        <div class="col-12">
            <div class="d-flex justify-content-between">
                <h1 data-cy="activity-title" class="font-custome-36 fw-custome-700">Activity</h1>
                <button class="btn bg-custome-blue-shoft rounded-pill px-3 text-white font-custome-18 fw-custome-600" @click="postActivity('activity-groups', {'title': 'New Activity'})" data-cy="activity-add-button">
                    <img src="../assets/icons/plus.svg" alt="add" width="24">
                    <span class="ms-2">Tambah</span>
                </button>
            </div>
        </div>
        <!-- list activity -->
        <div class="col-12">
            <List :showLoading="isLoading" ref="ListActivity" @delete:Activity="openModalDelete($event)"/>
        </div>
    </div>

    <!-- modal delete activity -->
    <div class="modal fade" id="deleteActivity" tabindex="-1" :aria-labelledby="`deleteActivityLabel`" aria-hidden="true" data-cy="modal-delete">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content rounded-12 border-0">
                <div class="modal-body border-0">
                    <div class="pt-4">
                        <img class="mx-auto d-block" src="../assets/icons/warning.svg" alt="warning" width="84" data-cy="modal-delete-icon">
                    </div>
                    <div class="desc text-center my-4" data-cy="modal-delete-title">
                        <span class="font-18 fw-custome-500 text-custome-black">
                            Apakah anda yakin menghapus activity
                        </span> 
                        <br>
                        <span class="font-18 fw-custome-700 text-custome-black">
                            “{{activityName}}”?
                        </span>
                    </div>
                </div>
                <div class="modal-footer justify-content-center border-0">
                    <button type="button" class="btn bg-custome-gray-shoft rounded-pill px-4 font-18 text-custome-gray-strong fw-custome-600" id="closeModalDelete" data-bs-dismiss="modal" data-cy="modal-delete-cancel-button">Close</button>
                    <button type="button" class="btn bg-custome-red rounded-pill px-4 font-18 text-white fw-custome-600" @click="deleteActivity(`activity-groups/${idActivity}`)" data-cy="modal-delete-confirm-button">Hapus</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import { mapActions } from 'vuex';
    import List from "../components/dashboard/List.vue"

    export default {
        components:{List},
        data(){
            return{
                isLoading: false,
                activityName: null,
                idActivity: null,
            }
        },

        methods: {
            ...mapActions({
                postData: 'postData',
                deleteData: 'deleteData'
            }),
    
            async postActivity(url, params){
                this.isLoading = true

                let data = {url, params}
                const response = await this.postData(data)
                
                if (response.status == 201 && this.$refs.ListActivity) {
                    // child modal
                    this.$refs.ListActivity.deleteAction = false
                    $('#deleteActivityNotif').modal('show')
                    this.$refs.ListActivity.getActivity('activity-groups')
                    setTimeout( ()=>{$('#deleteActivityNotif').modal('hide')}, 1000)
                }
            },

            openModalDelete(data){
                this.activityName = data.name;
                this.idActivity = data.id;

                $('#deleteActivity').modal('show')
            },

            async deleteActivity(url, params){
                this.isLoading = true

                let data = {url, params}
                const response = await this.deleteData(data)

                if (response.status == 200) {
                    $('#deleteActivity').modal('hide')
                    $('#deleteActivityNotif').modal('show')

                    this.$refs.ListActivity.getActivity('activity-groups')
                    
                    setTimeout(() =>{
                        $('#deleteActivityNotif').modal('hide')
                    },1000)

                }
            },
        },
    }
        
</script>