<template>
	<div id="app">
		<LeftSlip :list="list" @on-change="onChangeList(list)">
		</LeftSlip>
	</div>

</template>
<script>
	import LeftSlip	from './components/leftSlip.vue'
	export default {
		data(){
			return {
				list:[
					{
						text:'AAA',
						txtStyle:''
					},
					{
						text:'BBB',
						txtStyle:''
					},
					{
						text:'CCC',
						txtStyle:''
					}
				]
			}
		},
		components: {
			LeftSlip
		},
		methods:{
			onChangeList (list) {
				this.list = list;
			}
		}
	}
</script>
<style>
*{
	margin: 0;
	padding: 0;
}
</style>