<template>
	<view>
		<view>
		       
			</view>
		
		<view class="row">
			  
		</view>
		  
		  <robby-image-upload v-model="imageData" @delete="deleteImage" @add="addImage" :limit="limitNumber"></robby-image-upload>
		  
		<u--textarea v-model="c1" placeholder="请输入话题描述" autoHeight style="height: 200rpx;"></u--textarea>
		<tm-button  theme="bg-gradient-orange-accent" :round="24" block style="margin-top: 30rpx;" @click="rightClick">创 建</tm-button>
		
	</view>
</template>

<script>
	import robbyImageUpload from '@/components/robby-image-upload/robby-image-upload.vue';
	import tmButton from '@/tm-vuetify/components/tm-button/tm-button.vue';
	export default {
		components: {robbyImageUpload,tmButton},
		data() {
			return {
				c1:'',
				fileList1:[],
				tempimage:[],
				TopicID:'',
				imageData:[],
				
				enableDel : false,
                enableAdd : false,
                enableDrag : false,
                limitNumber: 9,
				formData: {
                    
                }
			};
		},
		methods:{
			rightClick(){
				let that = this
					var tmplist=[]
					let len=this.imageData.length
					console.log(len)
					for(let i=0;i<len;++i){
						var obj={
							'name':'PostPic',
							'uri':this.imageData[i]
						}	
						tmplist.push(obj)
					}
					console.log(tmplist)
					return new Promise((resolve, reject) => {
						uni.uploadFile({
							url: 'http://101.37.175.115/api/Community/Post/Create/'+'?token='+that.useMsg.token, // 仅为示例，非真实的接口地址
							files: tmplist,
							formData: {
								TopicID : this.TopicID,
								HasImage : 'True',
								Content	: this.c1,
								
							},
							success: (res) => {
								setTimeout(() => {
									resolve(res.data)
								}, 1000)
								console.log(res.data)
								uni.navigateTo({
									url:'./topic'
								})
							},
							fail: (err) => {
								console.log(this.TopicID)
								console.log(this.c1)
								console.log('fail')
								console.log(err)
							}
						});
					})
				},
			deleteImage: function(e){
                console.log(e)
            },
            addImage: function(e){
                console.log(e)
            },
			getToken(){	//获取token
				return new Promise((req,rej)=>{
					uni.getStorage({
						key: 'userMsg',
						success: function (res) {
							req(res.data)
						},
					})
				})
			},
			getTopicID(){
				return new Promise((req,rej)=>{
					uni.getStorage({
						key:'TopicID',
						success:function(res){
							req(res.data)
							// console.log(res.data.TopicID)
						}
					})
				})
			},
		},
		async onLoad() {
			this.useMsg = await this.getToken()
			this.Topic = await this.getTopicID()
			this.TopicID = this.Topic.TopicID
			// console.log(this.TopicID)
		}
	}
</script>

<style lang="scss">
	.row{
		display: flex;
		flex-direction: row;
		margin-top: 15rpx;
	}
</style>
