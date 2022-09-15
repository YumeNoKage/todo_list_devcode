<template>    
    <div class="row">
        <div class="col-12 d-flex justify-content-center mt-5 pt-3" v-if="data.length == 0 && !isLoading" data-cy="activity-empty-state">
            <img src="../../assets/illustration/activity-empty-state.svg" alt="activity empty state" data-cy="activity-empty-state">
        </div>
        <div class="col-12 mt-5 pt-3" v-if="isLoading">
            <Loading />
        </div>
        <div class="col-12 mt-5 pt-3" v-else-if="data.length > 0 && !isLoading">
            <div class="row">
                <div class="col-lg-3 col-md-4 col-sm-12 mb-4 position-relative text-decoration-none" v-for="(item, index) in data" :key="index">
                    <router-link :to="`/detail/${item.id}`" class="card border-0 custome-box-shadow rounded-12 cursor-pointed text-decoration-none text-custome-black" data-cy="activity-item" :id="`item_${index}`" :key="index" >
                        <div class="card-body d-flex align-items-end flex-column" style="min-width: 235px; min-height: 234px;">
                            <h5 class="card-title font-18 fw-custome-700 w-100" data-cy="activity-item-title">{{item.title}}</h5>
                            <div class="d-flex justify-content-between mt-auto w-100">
                                <span class="font-14 text-custome-gray-medium" data-cy="activity-item-date">{{ formatDate(item.created_at,'DD MMMM yyyy') }}</span>
                            </div>
                        </div>
                    </router-link>
                    <button class="p-0 border-0 bg-transparent position-absolute bottom-0 end-0 pe-1 mx-4 mb-3" data-bs-target="#deleteActivity" data-bs-toggle="modal" @click="updateToParent(item.id, item.title), deleteAction = true" data-cy="activity-item-delete-button">
                        <img src="../../assets/icons/trash.svg" alt="delete" width="24">
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- modal information -->
    <div class="modal fade" id="deleteActivityNotif" tabindex="-1" aria-labelledby="deleteActivityNotifLabel" aria-hidden="true" data-cy="modal-information">
        <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content rounded-12 border-0">
                <div class="modal-body border-0">
                    <div class="desc text-left">
                        <span class="font-14 fw-custome-500 text-custome-black">
                            <img src="../../assets/icons/info.svg" class="to-red">
                            <!-- Activity berhasil {{deleteAction ? 'dihapus' : 'ditambahkan'}} -->
                            Activity berhasil dihapus
                        </span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import { mapActions } from 'vuex';

    export default {
        props:{
            showLoading: {default: false},
        },
        data(){
            return{
                data:[],
                isLoading: this.showLoading,
                idActivity: null,
                activityName: null,
                deleteAction: true,
            }
        },

        emits:['delete:Activity'],
        
        methods: {
            ...mapActions({
                getData: 'getData',
                deleteData: 'deleteData'
            }),

            async getActivity(url, params){
                this.isLoading = true

                let data = {url, params}
                const response = await this.getData(data)

                if (response.status == 200) {
                    this.data = response.data.data
                    this.isLoading = false
                }
            },

            updateToParent(id, name){
                let data ={id, name}
                this.$emit('delete:Activity', data)
            }
        },

        mounted(){
            this.getActivity('activity-groups')
        }
    }
</script>