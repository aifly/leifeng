<template>
	<div :style="{background:'url('+imgs.hotwordsBg+') no-repeat center bottom',position:'fixed',height:viewH+'px',backgroundSize:'cover'}"  :class="{'show':show}"  class="zmiti-form-main-ui lt-full" ref='page'>
		<div class="zmiti-info">
			<img :src="imgs.info" alt="">
		</div>
		<h2>我们会为您保密哦</h2>

		<div class="zmiti-form">
			<div class="zmiti-form-item">
				<h3>您的性别:</h3>
				<div class='zmiti-input'>
					<span>{{sexName}}</span>
					<select v-model='sex'>
						<option :value="-1">--请选择--</option>
						<option :value="0">男</option>
						<option :value="1">女</option>
					</select>
				</div>
			</div>
			<h4></h4>
			<div class="zmiti-form-item">
				<h3>您的年龄:</h3>
				<div class='zmiti-input'>
					<span>{{ageName}}</span>
					<select v-model='age'>
						<option :value="age.value" v-for='age in ageList'>{{age.label}}</option>
					</select>
				</div>
			</div>
			<h4></h4>
			<div class="zmiti-form-item">
				<h3>您的学历:</h3>
				<div class='zmiti-input'>
					<span>{{eduName}}</span>
					<select v-model='edu'>
						<option :value="edu.value" v-for='edu in eduList'>{{edu.label}}</option>
					</select>
				</div>
			</div>
			<h4></h4>
			<div class="zmiti-form-item">
				<h3>您所在的省份:</h3>
				<div class='zmiti-input'>
					<span>{{provinceName}}</span>
					<select v-model='province'>
						<option :value="province.value" v-for='province in provinceList'>{{province.label}}</option>
					</select>
				</div>
			</div>

			<div class="zmiti-form-submit" v-tap='submitInfo'>提交</div>
			
		</div>
		<Toast :msg='showToastMsg'></Toast>
	</div>
</template>
<script>
	import './index.css';
	import imgs from '../lib/assets.js';
	import $ from 'jquery';
	import Toast from '../toast/toast.vue';
	export default {
		name:'zmitipage1',
		props:['obserable'],
		data(){
			return {
				imgs,
				play:false,
				showToastMsg:'',
				index:-1,
				show:false,
				words:[],
				age:-1,
				hotwordids:'',
				edu:-1,
				viewH:document.documentElement.clientHeight,
				sex:-1,
				sexName:'--请选择--',
				ageName:'--请选择--',
				eduName:'--请选择--',
				provinceName:'--请选择--',
				province:-1,
				ageList:[{
				    value: '-1',
				    label: '--请选择--',
				  },{
				    value: '0',
				    label: '20岁以下',
				  },{
				    value: '1',
				    label: '20-30岁',
				  },{
				    value: '2',
				    label: '30-40岁',
				  },{
				    value: '3',
				    label: '40-50岁',
				  },{
				    value: '4',
				    label: '50-60岁',
				  },{
				    value: '5',
				    label: '60岁以上',
				  }],
				  eduList:[
					{
				    value: '-1',
				    label: '--请选择--',
					  },
				  	{
					    value: '0',
					    label: '高中及以下',
					  },{
					    value: '1',
					    label: '本科',
					  },{
					    value: '2',
					    label: '硕士',
					  },{
					    value: '3',
					    label: '博士',
					  },{
					    value: '4',
					    label: '其它',
					  }
				  ],
				  provinceList:[
				  		{
				    value: '-1',
				    label: '--请选择--',
			  },
				  	{
						value: 1,
						label: '北京',
					},{
						value: 2,
						label: '天津',
					},{
						value: 3,
						label: '上海',
					},{
						value: 4,
						label: '重庆',
					},{
						value: 5,
						label: '河北',
					},{
						value: 6,
						label: '山西',
					},{
						value: 7,
						label: '辽宁',
					},{
						value: 8,
						label: '吉林',
					},{
						value: 9,
						label: '黑龙江',
					},{
						value: 10,
						label: '江苏',
					},{
						value: 11,
						label: '浙江',
					},{
						value: 12,
						label: '安徽',
					},{
						value: 13,
						label: '福建',
					},{
						value: 14,
						label: '江西',
					},{
						value: 15,
						label: '山东',
					},{
						value: 16,
						label: '河南',
					},{
						value: 17,
						label: '湖北',
					},{
						value: 18,
						label: '湖南',
					},{
						value: 19,
						label: '广东',
					},{
						value: 20,
						label: '海南',
					},{
						value: 21,
						label: '四川',
					},{
						value: 22,
						label: '贵州',
					},{
						value: 23,
						label: '云南',
					},{
						value: 24,
						label: '陕西',
					},{
						value: 25,
						label: '甘肃',
					},{
						value: 26,
						label: '青海',
					},{
						value: 27,
						label: '台湾',
					},{
						value: 28,
						label: '内蒙古',
					},{
						value: 29,
						label: '广西',
					},{
						value: 30,
						label: '西藏',
					},{
						value: 31,
						label: '宁夏',
					},{
						value: 32,
						label: '新疆',
					},{
						value: 33,
						label: '香港',
					},{
						value: 34,
						label: '澳门',
					}

				  ],
				chooseWords:[]
			}
		},
		
		watch:{
			sex(e,sex){
				this.sexName = e*1 === 0 ? '男':'女';
			},
			edu(e){
				this.eduList.forEach((edu,i)=>{
					if(edu.value === e){
						this.eduName = edu.label
						return;
					}
				})
			},
			age(e){
				this.ageList.forEach((age,i)=>{
					if(age.value === e){
						this.ageName = age.label;
						return;
					}
				})
			},
			province(e){
				this.provinceList.forEach((province)=>{
					if(province.value === e){
						this.provinceName = province.label;
						return;
					}
				})
			}
		},
		methods:{

			toast(msg='',time=2000){

				this.showToastMsg = msg;
				setTimeout(()=>{
					this.showToastMsg = '';
				},time)

			},

			submitInfo(){
				console.log(this.age,this.edu,this.province,this.sex)

				var s = this;
				if(this.sex === -1){
					this.toast('请选择性别');
					return;	
				}
				if(this.age === -1){
					this.toast('请选择年龄');
					return;
				}
				if(this.edu === -1){
					this.toast('请选择学历');
					return;	
				}
				if(this.province === -1){
					this.toast('请选择省份');
					return;	
				}

				var {obserable} = this;
				
				
				$.ajax({
					type:"post",
					url:window.protocol+"//api.zmiti.com/v2/h5/post_hotwordinfo",
					data:{
						'worksclassid':3,
						'sex':s.sex,
						'proviceid':s.province,
						'userage':s.age,
						'education':s.edu,
						'hotwordids':s.hotwordids,
					},
					error(){
						s.toast('服务器错误');
					},
					success(data){
						console.log(data);
						if(data.getret==0){
							s.toast('提交成功!!');
							obserable.trigger({
								type:'showShareApp'
							})
						}else{
							s.toast('提交失败!!');
						}
					}
					
				})

			}
			
		},
		components:{
			Toast
		},
		mounted(){
			
			var {obserable} = this;

			/**/

			obserable.on('showFormApp',()=>{
				this.show = true;
			})


			obserable.on('setHotwordsid',(data)=>{
				this.hotwordids = data;
			})

			


			
		}
	}
</script>