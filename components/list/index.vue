<template>
	<div :style="{background:'url('+imgs.hotwordsBg+') no-repeat center bottom',backgroundSize:'cover'}"  :class="{'show':show}" class="zmiti-list-main-ui lt-full" ref='page'>
		<div class="zmiti-vote">
			<img :src="imgs.vote" />
		</div>

		<div class="zmiti-words-list">
			
			<div ref='word' class="zmiti-word-item" v-for='(word,i) in words' :style='word.style'>
				<div>
					<div>{{hotWords[0] && hotWords[i].hotword}}</div>
					<div>{{hotWords[0] && hotWords[i].scale}}</div>
				</div>
			</div>

			<div class="zmiti-hotword">
				<img :src="imgs.hotwords" alt="">
			</div>

		</div>

		<div class="zmiti-back" v-tap='hideList'>返回</div>
	</div>
</template>
<script>
	import './index.css';
	import ZmitiMove from '../lib/move';
	import imgs from '../lib/assets.js';
	import $ from 'jquery';
	export default {
		name:'zmitipage1',
		props:['obserable'],
		data(){
			return {
				imgs,
				play:false,
				index:-1,
				show:false,
				viewH:document.documentElement.clientHeight,
				hotWords:[],
				words:[
					{
						style:{
							width:'3rem',
							height:'3rem',
							left:'6.3rem',
							top:'3.5rem',
							background:'#63b7f5'
						}
						
					},{
						style:{
							width:'2.8rem',
							height:'2.8rem',
							left:'1.5rem',
							top:'.5rem',
							background:'#f05553'
						}
					},{
						style:{
							width:'2.6rem',
							height:'2.6rem',
							left:'1.5rem',
							top:'4.5rem',
							background:'#ffa509'
						}
					},{
						style:{
							width:'2.4rem',
							height:'2.4rem',
							left:'4rem',
							top:'.6rem',
							background:'#36c15a'
						}
					},{
						style:{
							width:'2.1rem',
							height:'2.1rem',
							left:'3.4rem',
							top:'6rem',
							background:'#6ce8b4',
							fontSize:'.4rem'
						}
					},{
						style:{
							width:'2rem',
							height:'2rem',
							left:'5.7rem',
							top:'6.2rem',
							background:'#ffc116',
							fontSize:'.4rem'
						}
					},{
						style:{
							width:'2rem',
							height:'2rem',
							left:'.1rem',
							top:'4rem',
							background:'#fdca01',
							fontSize:'.4rem'
						}
					},{
						style:{
							width:'1.9rem',
							height:'1.9rem',
							left:'.1rem',
							top:'2rem',
							background:'#f9e6ac',
							color:'#f57700',
							fontSize:'.4rem'
						}
					},{
						style:{
							width:'1.8rem',
							height:'1.8rem',
							left:'6.3rem',
							top:'2rem',
							background:'#2b97f1',
							fontSize:'.4rem'
						}
					},{
						style:{
							width:'1.7rem',
							height:'1.7rem',
							left:'8rem',
							top:'2rem',
							background:'#f7e6ae',
							color:'#f57700',
							fontSize:'.38rem'
						}
					},{
						style:{
							width:'1.7rem',
							height:'1.7rem',
							left:'7.7rem',
							top:'6.5rem',
							background:'#f7e6ae',
							color:'#f57700',
							fontSize:'.38rem'
						}
					},{
						style:{
							width:'1.7rem',
							height:'1.7rem',
							left:'6.7rem',
							top:'.2rem',
							background:'#f7e6ae',
							color:'#f57700',
							fontSize:'.38rem'
						}
					},{
						style:{
							width:'1.6rem',
							height:'1.6rem',
							left:'.2rem',
							top:'6.2rem',
							background:'#f05553',
							fontSize:'.32rem',
						}
					},{
						style:{
							width:'1.6rem',
							height:'1.6rem',
							left:'.2rem',
							top:'.2rem',
							background:'#f05553',
							fontSize:'.32rem',
						}
					},{
						style:{
							width:'1.6rem',
							height:'1.6rem',
							left:'1.8rem',
							top:'7.4rem',
							background:'#36c15a',
							fontSize:'.32rem',
						}
					}
				],
			}
		},
		methods:{
			hideList(){
				this.start = false;
				this.show = false;
			}
		},
		components:{
		},
		mounted(){
			
			var {obserable} = this;

			/*obserable.trigger({
				type:'setPlay',
				data:{
					top:'.5rem !important'
				}
			});*/
			

			var wordArr = [];
			this.words.forEach((word,i)=>{
				wordArr.push(new ZmitiMove({
					obj:this.$refs['word'][i]
				}))
			})

			var zmitiRequetAnimationFrame = window.requestAnimationFrame || window.webkitRequestAnimationFrame


			this.start = false;
			var render =  ()=>{
				wordArr.forEach((word)=>{
					word.move();
				})

				this.start && zmitiRequetAnimationFrame(render);
			}

			render()


			obserable.on('showHotwordsList',()=>{
				this.start = true;
				this.show = true;
				render();

				$.ajax({
					url:'http://api.zmiti.com/v2/h5/count_hotword',
					type:'post',
					data:{
						worksclassid:3
					}
				}).done((data)=>{
					var allCount =0;
					data.list.map((ls)=>{
						allCount += ls.num*1;
					});

					
					data.list.forEach(data=>{
						data.scale = (100*data.num / allCount).toFixed(1) +'%'
					})
					
					if(data.getret === 0){
						this.hotWords = data.list;
					}
				})
			})

			//obserable.trigger({type:'showHotwordsList'})

			/**/
			
		}
	}
</script>