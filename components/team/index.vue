<template>
	<div :style="{background:'#158ae4',position:'fixed',height:viewH+'px'}"  :class="{'show':show}" class="zmiti-team-main-ui lt-full" ref='page'>
		<img style='position:absolute;z-index:0;' :src="imgs.loading1" alt="" />

		<div class="zmiti-team-content">
			<h1>新华社新媒体中心出品</h1>

			<div>
				<span>出品：</span><span>陈凯星</span><span>冯瑛冰</span><span>马书平</span>
			</div>
			<div>
				<span>监制：</span><span>齐慧杰</span>
			</div>
			<div>
				<span>策划：</span><span>黄庆华</span>
			</div>
			<div>
				<span>方案：</span><span>孟洁</span><span>刘雅萱</span>
			</div>
			<div>
				<span>制作：</span><span>马发展</span><span>麟腾传媒</span>
			</div>
		</div>

		<div class="zmiti-back" v-tap='back'>返回</div>


	</div>
</template>
<script>
	import './index.css';
	import imgs from '../lib/assets.js';

	//Vue.use(VueAnimatedList)
	export default {
		name:'zmitipage2',
		props:['obserable'],
		data(){
			return {
				imgs,
				show:false,
				viewH:document.documentElement.clientHeight
			}
		},
		methods:{
			back(){
				this.show = false;
			}
		},
		components:{
		},
		mounted(){
			
			var {obserable} = this;

		/*	obserable.trigger({
				type:'setPlay',
				data:{
					top:'.5rem !important'
				}
			});*/

			obserable.on('showTeamApp',(data)=>{
				
				this.show = true;

				
			})

			/*setTimeout(()=>{

				this.words = obserable.trigger({type:'getWords'})

			},400)*/
		}
	}
</script>