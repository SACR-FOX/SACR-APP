 <template>
	<view>
		
		
		<u-popup :show="show" closeOnClickOverlay @close="show = false">
			<u-input v-model="tmpName"  placeholder="请输入项目名称"></u-input>
			<view style="display: flex; align-items: center; justify-content: center; height: 200rpx;">
				<text @click="timePickerShow = true" style="font-weight: bold; font-size: 50rpx;">{{tmpTime}}</text>
			</view>
			<u-datetime-picker :show="timePickerShow" mode="time" v-model="tmpTime" 
								@cancel="timePickerShow = false" @confirm="timePickerShow = false"></u-datetime-picker>
								
			<button @click="add()">确认</button>					
		</u-popup>
		
		<view class="top">
			
			<circle-progress-bar :pro="poject_now.percent/100" 
								  :size="500" :border_width = "30"
								  border_color = "#faa200" border_back_color="#fae9ca"
								  animate="">
				<view class="timer_center">
					<text style="font-size: 15px; font-weight: bold;">{{poject_now.name}}</text>
					<text style=" font-weight: bold;" :style="{'font-size' : fontSize}">{{timeChange(poject_now.date)}}</text>
					<text  style="font-size: 15px; font-weight: bold;">{{poject_now.percent}}%</text>
				</view>
			</circle-progress-bar>
		</view>
		<view class="control_bar">
			<view class="tabbar" style="margin-right: 110px;" @click="counterEnd()">
				<u-icon :name="icon[icon_flag]" color="#55aaff" size="60px"></u-icon>
				<text style="font-size: 20px; font-weight: bold;">{{icon_text[icon_flag]}}</text>
			</view>
			<view class="tabbar" @click="counterStart()">
				<u-icon name="play-circle"  color="#55aaff" size="60px"></u-icon>
				<text style="font-size: 20px; font-weight: bold;">开始</text>
			</view>
		</view>
		<view class="mention">
			<text style="font-size: 20px; font-weight: bold; margin: 10px;">今天已完成</text>
			<text style="font-size: 30px; font-weight:bold ;" :style="{'color' : '#55aaff'}">{{Math.floor(learning_time/3600)}}</text>
			<text  style="font-size: 20px; font-weight: bold;  margin: 10px;">小时</text>
			<!-- <u--text :text = "'今天已完成' + learning_time + '小时'" bold size = "25" style="margin-left:110px;"></u--text> -->
		</view>
		<view class="history">
			<u--text text = "历史定时" size = "18" bold style="margin-left: 20px;"></u--text>
				<scroll-view class="history_list"  scroll-y="true">
					<view v-for="(item,index) in history_list" >
						<view class="item" @click="workChanger(item)" :style="{backgroundColor : color_list[item.color] }">
							
							<!-- <u--text :text = "item.name" style="margin-left: 20px; width: 150px;" bold size = "20"></u--text> -->
							<text class="itemText" style="width: 170px;">{{item.name}}</text>
							<!-- <u--text :text = "item.date" bold size = "20" style="margin-left: 20px;"></u--text> -->
							<text  class="itemText" style="width: 70px;">{{timeChange(item.date)}}</text>
							<!-- <u--text :text = "item.percent" bold size = "20"></u--text> -->
							<text  class="itemText">{{item.percent}}%</text>
						</view>
					</view>
				</scroll-view>
		</view>
		<button @click="addWork()">添加任务</button>
	</view>
</template>

<script>
	
	
	export default {
		data() {
			return {
				
				"show" : false,
				timePickerShow : false,
				
				tmpTime : 0,
				tmpName : "",
				
				"learning_time" : 7200,
				"history_list" : [
					{
						"name" : "Test1",
						"date" : 3600,
						"percent" : 100,
						"color" : 1,
					},
					{
						"name" : "Test2",
						"date" : 1800,
						"percent" : 100,
						"color" : 0,
					},
					{
						"name" : "Test3",
						"date" : 1600,
						"percent" : 100,
						"color" : 1,
					},{
						"name" : "Test4",
						"date" : 200,
						"percent" : 100,
						"color" : 2,
					},
					],
				
				//显示当前的计划
				"poject_now" : {
					"name" : "当前没有任务",
					"date" : 0,
					"percent" : 0,
					"color" : 0,
				},
				
				//当前是否有任务控制
				work_flag: false,
				
				//计时器控制
				"fontSize" : "50px",
				"color_list" : ["#b1d522","#ff666b","#ff9718"],
				pro : 1,
				"timer_flag" : false ,//控制暂停开始
				"timer" : null,
				"date" : 0,
				//控制暂停icon
				icon : ['pause-circle','checkmark-circle'],
				icon_flag : 0,
				icon_text :  ['暂停','结束'],
				//
				"timer_color" : {},
				
				count : 0,
				
				//开始按钮控制
				starButton : true,
				
				userMsg : {}
			}
		},
		methods: {
			
			//切换任务
			workChanger(item){
				//this.counterEnd()
				if(!this.work_flag){
					this.work_flag=!this.work_flag
					this.poject_now = item
					// this.pro = item.percent/100
					this.date = this.poject_now.date
					console.log(item.name)
				}else{
					console.log("请先结束")
				}
				
			},
			
			addWork(){
				let that = this
				that.show =true
				that.tmpName = ""
				that.tmpTime = "请选择时间"
			},
			
				
			add(){
				if(this.tmpName != "" && this.tmpTime != "请选择时间"){
					let data ={
						"name" : this.tmpName,
						"date" : this.strToTime(this.tmpTime),
						"percent" : 100,
						"color" : Math.floor((Math.random()*3)),
					}
					this.history_list.push(data)
					console.log(data.date)
					
					this.show = false
				}else{
					uni.showToast({
						title: "请输入",
						icon: "error"
					})
				}
				
			},
			
			
			strToTime(time){
				let tmp = time.split(":")
				return parseInt(tmp[0]*3600)+parseInt(tmp[1]*60)
			},
			
			//倒计时
			async counter(){
				this.date = this.poject_now.date/(this.poject_now.percent/100)
				// console.log(this.date)
				let that = this
				let per = that.poject_now.percent
				// console.log(per)
				// console.log(this.poject_now.date)
				if(that.poject_now.date == 0 || !that.timer_flag){
					this.work_flag = false
					console.log(this.count)
					await this.sendEXP(this.count)
					this.count = 0
					console.log("end")
					clearTimeout(this.timer)
				}else{
					
					this.poject_now.date = this.poject_now.date -1
					this.poject_now.percent = Math.round((this.poject_now.date/(that.date))*100)
					this.count+=1
					this.learning_time +=1
					console.log(this.count)
					setTimeout(that.counter,1000)
				}
			},
			
			sendEXP(time){
				return new Promise((req,rej)=>{
					uni.request({
						url: "http://101.37.175.115/api/Reward/Add/?token="+this.userMsg.token,
						method:"POST",
						data:{
							ItemName : "Timer",
							Durations : time.toString(),
						},
						success: (res) => {
							console.log(res.data)
							req("success")
						},
						fail: (err) => {
							console.log(err)
							rej("err")
						}
					})
				})
			},
			
			rightClick(){
					uni.navigateTo({
						url:"../index/index"
					})
				},
			
			counterStart(){
					
				if(!this.work_flag){
					uni.showToast({
						title: "请选择任务",
						icon: "error"
					})
					return 0
				}
				this.timer_flag = true
				this.icon_flag = 0
				clearTimeout(this.timer)
				this.timer = null
				if(this.starButton){
					this.timer = setTimeout(this.counter(),1000) 
					this.starButton = false
					
				}else{
					uni.showToast({
						title: "请不要重复点击",
						icon: "error"
					})
				}
				
			},
			
			counterEnd(){
				let that = this
				if(this.icon_flag == 0){
					this.timer_flag = false
					this.icon_flag = 1
					this.starButton = true
					this.timer = null
					
				}else{
					this.icon_flag = 0
					this.work_flag = false
					that.poject_now = {
						"name" : "当前没有任务",
						"date" : 0,
						"percent" : 0,
						"color" : 0,
					}
				}
			},
			
			//时间转换
			timeChange(time){
				let that = this
				var timeC = ""
				if(time <= 60){
					timeC = "00:" + time.toString()
					return timeC
				}else if(time < 3600){
					let min = (Math.floor(time/60)).toString()
					let sec = (time%60).toString()
						
					if(min.length <= 1){
						min = "0"+min
					}
					if(sec.length <= 1){
						sec = "0"+sec
					}
					timeC = min + ":" + sec
					return timeC
				}else{
					let hour =  (Math.floor(time/3600)).toString()
					let min = (Math.floor((time%3600)/60)).toString()
					let sec = ((time%3600)%60).toString()
					that.fontSize = "45px"
					if(hour.length <= 1){
						hour = "0" + hour
					}
					if(min.length <= 1){
						min = "0" + min
					}
					if(sec.length <= 1){
						sec = "0"+sec
					}
					timeC = hour + ":"+ min + ":" + sec
					return timeC
				}
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
			this.strToTime("02:00")
			await this.getUserMsg()
			console.log(this.userMsg.token)
			
			
		},
		
		onReady() {
			
		},
		onHide() {
			
		}
	}
</script>

<style>
		
	.itemText{
		font-size: 20px;
		font-weight: bold;
		margin: 20px;
		color: #F1F1F1;
		text-shadow: 0 0 5px #000000, 0 0 5px #000000;
		
	}
	.history_list{
		height: 250px;
	}
	.timer_center{
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: space-around;
		
	}
	.top{
		background-color: #F1F1F1;
		height: 400px;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	
	.control_bar{
		background-color: #F1F1F1;
		height: 100px;
		display: flex;
		flex-direction: row;
		justify-content: center;
	}
	
	.mention{
		background-color: #F1F1F1;
		height: 70px;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
	}
	
	.history{
		background-color: #F1F1F1;
		height: 275px;
	}
	.tabbar{
		margin: 20px;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	
	.item{
		height: 70px;
		background-color: #000000;
		margin-top: 10px;
		display: flex;
		flex-direction: row;
		align-items: center;
	}
</style>
