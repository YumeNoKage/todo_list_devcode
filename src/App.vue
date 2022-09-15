<template>
    <div id="nav" class="bg-custome-blue-shoft" data-cy="header-background" >
		<div class="container py-4">
			<h4 data-cy="header-title" class="text-uppercase text-white fw-custome-700 my-3">TO DO LIST APP</h4>
		</div>
	</div>
    <router-view/>
</template>

<script>
export default {
	created(){
		$('.modal-backdrop.fade.show').on('click', ()=>{
			$('.modal-backdrop.fade.show').removeClass('show')
		})
	}
}
</script>