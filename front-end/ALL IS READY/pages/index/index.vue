<template>
	<view>
		<view class="card_top" style="display: flex; ">
			<view style="display: flex;flex-direction: row;margin-top: 60rpx;">
				<u-avatar src="../../static/avatar.jpg" :src="user.Header" style="margin-left: 50rpx; margin-top: 15rpx;" size="50"></u-avatar>
				<view style="">
					<view style="text-align: right; margin-left: 170px;display: flex;">
						<text style="margin-top: 30rpx;">今天学习了:</text>
						<text style="font-size: 60rpx; 
									margin-left: 15rpx;margin-right: 15rpx;
									font-weight: bold;">{{timeChange(user.Accumulation)}}</text>
						<text style="margin-top: 30rpx;">{{timeList[timeFlag]}}</text>
					</view>
					<view style="text-align: right;">TOP:{{Situation.percent}}%</view>
				</view>
			</view>
		</view>
		
		<view class="content">
			<view class="col">
				<view class="card" @click="toTodolist()">
					<view style="display: flex; flex-direction: column; margin-left: 55rpx;margin-top: 50rpx;">
						<text style="font-size: 30rpx; font-weight: bold; color: #E43D33;margin-left: 10rpx;">{{week[date.week]}}</text>
						<view>
							<text style="font-size: 75rpx; font-weight: bold; margin-top: 10rpx;">{{date.day}}</text>
							<text style="font-size: 30rpx; font-weight: bold; margin-top: 10rpx; margin-left: 20rpx;">{{mon[date.mon]}}.</text>
						</view>
						<text style="font-size: 40rpx; font-weight: bold; margin-top: 30rpx;margin-left: 0rpx;">{{textFix(todoList,6)}}</text>
					</view>
				</view>
				
				<view class="card" @click="toClassSche()"> 
					<view  style="display: flex; flex-direction: column; margin-left: 55rpx;margin-top: 50rpx;">
						<text style="font-weight: bold;">下一节</text>
						<text style="font-size: 60rpx; font-weight: bold; margin-top: 10rpx;">{{textFix(classSch.CurName,3)}}</text>
						<text style="font-weight: bold; margin-top: 20rpx;">{{classSch.Location}}</text>
						<text style="font-weight: bold;">{{timeToStr(classSch.Start)}}</text>
					</view>
				</view>
				
			</view>
		</view>
		
		<view class="content">
			<view class="col">
				
				<view class="card" style="align-items: center;justify-content: center;" @click="toTimer()">
					<view>
						<circle-progress-bar :pro="pro" :size="230" 
						:border_width="20" :animate="true"
						>
							<text style="font-size: 46rpx;">
								{{pro * 100}}%
							</text>
						</circle-progress-bar>
					</view>
				</view>
				
				<view class="card" @click="toLeaderB()">
					<view style="display: flex; flex-direction: column; margin-left: 55rpx;margin-top: 50rpx;">
						<text style="font-weight: bold; font-size: 30rpx;">班级动态</text>
						<text style="font-weight: bold; font-size: 60rpx; margin-top: 60rpx;">{{textFix(classAct,4)}}</text>
					</view>
				</view>
				
			</view>
		</view>
		
		<view class="card_bottom" @click="toFile()">
			<scroll-view scroll-x="true" style="flex-direction: row; white-space: nowrap;">
				<view v-for="(item,index) in fileList" class="card_doc">
					<view style="display: flex; flex-direction: column;">
						<image src="../../static/pdf_icon.png"
						style="width: 150rpx; height: 150rpx; margin: 10rpx;"
						mode="scaleToFill"></image>
						<text style="font-weight: bolder;margin-left: 15rpx;">{{textFix(item.name,6)}}</text>
						<text style="font-weight: bolder;margin-left: 15rpx;">{{item.size}}M</text>
					</view>
				</view>
			</scroll-view>
		</view>
		<!-- <button @click="j">11</button> -->
		<!-- <view style="display: flex; flex-direction: column;">
			<view @click="j"
			style="background-color: #0d0d0d;height: 200rpx; margin: 20rpx; border-radius: 15rpx;"> 
				
			</view>
			<view style="background-color: #F1F1F1;height: 200rpx; margin: 20rpx; border-radius: 15rpx;">
				
			</view>
		</view> -->
		<view class="content">
			<view class="col">
				
				<view class="card" style="align-items: center;justify-content: center;" @click="j()">
					<view style="display: flex; flex-direction: column; font-weight: bold; font-size: 50rpx; align-items: center;justify-content: center;">
						<image style="height: 100rpx; width: 100rpx;" src="../../static/com.png"></image>
						<text>班级社区</text>
					</view>
				</view>
				
				<view class="card"  style="align-items: center;justify-content: center;" @click="toOrg()">
					<view style="display: flex; flex-direction: column; font-weight: bold; font-size: 50rpx; align-items: center;justify-content: center;">
						<image style="height: 100rpx; width: 100rpx;" src="../../static/org.png"></image>
						<text>群组</text>
					</view>
				</view>
				
			</view>
		</view>
		
		<button @click="logout()"  style="background-color: #ffa4a4; margin-left: 15rpx;margin-right: 15rpx; font-weight: bold; color: #F1F1F1;">退出登录</button>

	</view>
</template>

<script>
	export default {
		data() {
			return {
						//token
						// token  : "eyJ0eXAiOiJqd3QiLCJhbGciOiJIUzI1NiJ9.eyJVSUQiOjIsIlVuYW1lIjoieHljZiIsIk9yZ0lEIjoxLCJleHAiOjE2NTMwOTczNDF9.UBTeUJOApg6Kd9rzKTDQKghsV8S0WA0aMGwYqt6Xk9E",
						// UID : "2",
						Url : "http://101.37.175.115/api/",
						userMsg : {},
						
						date : {},
						
						//用户信息
						user : {},
					
						//排名信息
						Situation : {},
						
						pro:0.6 ,//计时器
						
						//分钟为0，小时为1
						timeFlag : 0,
						timeList : ["分钟","小时"],
						
						//todoList
						week : ["星期日","星期一","星期二","星期三","星期四","星期五","星期六"],
						week_id : -1,
						days : -1,
						todoList : "",
						
						//学习时间
						learnTime : 6,
						
						//Jan、二月Feb、三月Mar、四月Apr、五月May、六月Jun、七月Jul、八月Aug、九月Sept、十月Oct、十一月Nov、十二月Dec。”
						mon : ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sept','Oct','Nov','Dec'],
						
						//classSchedule
						classSch : {
							"CurName" :"",
							"Start" : 0,
							"Location" : "",
							"Tag" : "rgba(255, 190, 118,1.0)",
							"End" : 0,
						},
						//班级动态
						classAct: "无事发生",
						
						//file
						fileList : [
							{
								'type' : 0,
								"name" : '编译原理白俄爱看啊阿斯顿阿萨原版书',
								'size' : 1.0
							},
							{
								'type' : 0,
								"name" : '编译原理原版书',
								'size' : 0.8
							},
							{
								'type' : 0,
								"name" : '编译原理原版书',
								'size' : 1.0
							},
							{
								'type' : 0,
								"name" : '编译原理原版书',
								'size' : 1.0
							},
						]
					}
		},
		methods: {
			j(){
				uni.navigateTo({
					url:'../community/community'
				})
			},
			toOrg(){
				uni.navigateTo({
					url:"../Organization/Organization"
				})
			},
			toTodolist(){
				uni.navigateTo({
					url:"../todo_list/todo_list"
				})
			},
			toClassSche(){
				uni.navigateTo({
					url:"../classSchedule/classSchedule"
				})
			},
			toTimer(){
				uni.navigateTo({
					url:"../timer/timer"
				})
			},
			toLeaderB(){
				uni.navigateTo({
					url:"../leaderBoard/leaderBoard"
				})
			},
			toFile(){
				uni.navigateTo({
					url:"../file/file"
				})
			},
			
			getTodoList(){
				let that = this
				return new Promise((req,rej)=>{
					
				})
			},
			
			logout(){
				uni.removeStorage({
					key:"userMsg",
					success: (res) => {
						console.log(res)
						uni.navigateTo({
							url:"../Login/Login"
						})
					}
				})
			},
			
			//修改过长字体
			textFix(text,length){
				
				if(text==undefined){
					return "无"
				}
				
				if(text.length <= length){
					return text
				}else{
					return (text.slice(0,length)+'...')
				}
			},
			
			timeChange(time){
				let that = this
				if(time<60){
					that.timeFlag = 0
					return 0
					
				}else if(time < 3600){
					that.timeFlag = 0
					return Math.floor(time/60)
					
				}else{
					that.timeFlag = 1
					return Math.floor(time/3600)
				}
			},
			
			timeToStr(time){
				let that = this;
				 // console.log(time)
				if(time == 0){
					return ""
				}
				if(time >= 3600){
					var hour = Math.floor((time/3600))
					// console.log(hour)
					var min = Math.floor(((time - hour * 3600))/60)
				}else{
					var hour = 0 
					var min = Math.floor(((time - hour * 3600))/60)
				}
				
				if(hour.toString().length < 2)
				{
					hour = "0"+hour
				}
				if(min.toString().length < 2)
				{
					min = "0"+min
				}
				return hour + ":" + min
			},
			
			
			getSystemInfo(){		
				uni.getSystemInfo({
					success: (res) => {
						console.log(res.deviceId)
					}
				})
			},
			
			getDate(){
				var date = new Date()
				
				var tmp = {
					year : date.getYear(),
					mon : date.getMonth(),
					day : date.getDate(),
					week : date.getDay()
				}
				
				this.date = tmp
			
			},
			
			
			getUserMsg(){
				return new Promise((req,rej) =>{
					uni.getStorage({
						key:"userMsg",
						success:(res)=>{
							console.log("success")
							this.userMsg = res.data
							req("success")
						},
						fail: (err) => {
							console.log("err")
							uni.reLaunch({
								url:"../Login/Login"
							})
						}
					})
				})
			},
		},
		async onLoad() {
			let that = this
			
			this.getDate()
			console.log(this.date.day)
			
			await this.getUserMsg()
			
			// console.log(this.userMsg.token)
				
			if(this.userMsg.length == 0){
				uni.reLaunch({
					url:"../Login/Login"
				})
			}
			
			uni.request({
				url: that.Url + "User/Detail/"+that.userMsg.UID+"?token="+that.userMsg.token,
				method: "GET",

				success: (res) => {
					that.user = res.data
					// console.log(that.user.Header)
				}
			})
			
			uni.request({
				url:that.Url + "Reward/Situation/" + "?token="+that.userMsg.token,
				success: (res) => {
					that.Situation = res.data
					// console.log(that.Situation.percent)
				}
			})
			
			uni.request({
				url:that.Url + 'ToDoList/Today?token=' + that.userMsg.token,
				success: (res) => {
					if(res.data.length == 0 || res.data == undefined){
						that.todoList = "目前没有事项"
					}else{
						that.todoList = res.data[0].ItemName
					}
					
					
				}
			})
			
			uni.request({
				url:that.Url + "Schedule/Next/?token="+that.userMsg.token,
				success: (res) => {
					 // console.log(res.data.CurName)
					if(res.data[0] == "result: Empty"){
						that.classSch = {
							"CurName" :"無",
							"Start" : 0,
							"Location" : "",
							"Tag" : "rgba(255, 190, 118,1.0)",
							"End" : 0,
						}
						 console.log("aaa")
					}else{
						that.classSch = res.data
						// console.log(res.data.Start)
						
					}
					// console.log(that.classSch.Start)
					// console.log(that.Url + "Schedule/Next/?token="+that.userMsg.token)
				}
			})
			
			// console.log(Math.floor(this.timeChange(3666)))
			// console.log(this.textFix("aaaaaa",3))
		},
		filters:{
			timeStamp: function(value) {
				var date = new Date(value); //时间戳为10位需*1000，时间戳为13位的话不需乘1000
				var year = date.getFullYear();
				var month = ("0" + (date.getMonth() + 1)).slice(-2);
				var sdate = ("0" + date.getDate()).slice(-2);
				var hour = ("0" + date.getHours()).slice(-2);
				var minute = ("0" + date.getMinutes()).slice(-2);
				// var second = ("0" + date.getSeconds()).slice(-2);
				// 拼接
				var result = year + "-" + month + "-" + sdate + " " + hour + ":" + minute ;
				// 返回
				return result;
			},
			
		}
	}
</script>

<style lang="scss">
	.content{
		width: 100%;
		height: auto;
		display: flex;
		align-items: center;
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: center;
		margin: 0;
		padding: 0 20rpx;
		box-sizing: border-box;
		
	}
	.col{
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		margin-top: 8px;
	}
	.card {
		width: 350rpx;
		height:350rpx;
		margin: 5px;
		background-color: rgba(241, 238, 236, 0.8);
		border-radius: 15px;
		display: flex;
		flex-direction: column;

	}
	.card_top{
		
		background-color: rgba(241, 238, 236, 0.8);
		border-radius: 30rpx;
		height: 200rpx;
	}
	.card_bottom{
		width: 700rpx;
		height: 300rpx;
		margin: 10px;
		background-color: rgba(241, 238, 236, 0.8);
		border-radius: 15px;
	}
	.card_doc{
		width: 260rpx;
		height: 260rpx;
		margin-top: 25rpx;
		margin-left: 25rpx;
		display: inline-block;
		background-color: rgba(255, 255, 255, 0.8);
		border-radius: 30rpx;
	}
	

</style>
