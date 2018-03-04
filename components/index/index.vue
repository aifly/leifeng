<template>
	<div  class="lt-full zmiti-index-main-ui "  :class="{'show':show}">

		<transition name="zmiti-scale"
			@after-enter="afterEnter"
		 >
		    <div class="zmiti-createimg" v-if='createImg'>
				<img :src="createImg" alt="">
			</div>
		  </transition>
		<div v-if='!createImg' :style="{background:'url('+imgs.bg+') no-repeat center top',backgroundSize:'cover'}" class="lt-full">
			<div class="zmiti-indx-bottom">
				<div class="zmiti-text">
					<select v-model='num'>
						<option  v-for='ct in count' :value="ct.value" v-html='ct.label'></option>
					</select>
					<h1>做新时代的雷锋</h1>
					<h1>今年我要做<span>
					<label>{{num}}</label>
					<div class="zmiti-hand" v-if='showHand'>
						<span></span>
						<span></span>
						<img :src="imgs.hand" alt="">
					</div>

				</span>件好事，立此为证。</h1>
					<div class="zmiti-logo-C">
						<div class="zmiti-qrcode">
							<img :src="imgs.qrcode" alt="">
						</div>
						<div class="zmiti-logo">
							<img :src="imgs.logo" alt="">
							<span>新华社新媒体中心出品</span>
						</div>
					</div>
				</div>
			</div>
		</div>


		<div v-if='!createImg' ref='zmiti-cache-page' :style="{background:'url('+imgs.bg+') no-repeat center top',backgroundSize:'cover',height:viewH*2+'px'}" class="zmiti-cache-page ">
			<div class="zmiti-indx-bottom">
				<div class="zmiti-text">
					<h1>做新时代的雷锋</h1>
					<h1>今年我要做<span>
					<label>{{num}}</label>
				</span>件好事，立此为证。</h1>
					<div class="zmiti-logo-C">
						<div class="zmiti-qrcode">
							<img :src="imgs.qrcode" alt="">
						</div>
						<div class="zmiti-logo">
							<img :src="imgs.logo" alt="">
							<span>新华社新媒体中心出品</span>
						</div>
					</div>
				</div>
			</div>
		</div>
		
		<div v-if='showBtns && !src' class="zmiti-share-btns">
			<div v-tap='restart'>重选</div>
			<div v-tap='showMask'>分享</div>
		</div>

		<div v-if='showBtns && src' class="zmiti-share-btns zmiti-share-btns1">
			<div v-tap='restart'>我也来承诺</div>
		</div>

		<div v-if='showMasks' @touchstart='hideMask' class="zmiti-mask">
			<img :src="imgs.arrow" alt="">
		</div>
	</div>
</template>

<script>
	import './index.css';
	import imgs from '../lib/assets.js';
	import zmitiUtil from '../lib/util';
	import '../lib/html2canvas';
	import $ from 'jquery';
	export default {
		props:['obserable'],
		name:'zmitiindex',
		data(){
			return{
				imgs,
				show:true,
				showTitle:false,
				viewW:window.innerWidth,
				showBtns:false,
				viewH:window.innerHeight,
				words:[],
				start:true,
				showMasks:false,
				count:[],
				showHand:true,
				num:'',
				createImg:'',
				src:''

			}
		},
		watch:{
			num(newVal,oldVal){
				this.html2img();
			}
		},
		methods:{
			hideMask(){
				this.showMasks = false;
			},
			showMask(){
				this.showMasks = true;
			},
			restart(){
				window.location.href = window.location.href.split('?')[0];
			},
			afterEnter(){
				this.showBtns = true;
			},
			html2img(){
				var s = this;
				var {obserable} = this;
				setTimeout(()=>{

					var ref = 'zmiti-cache-page';
					var dom = this.$refs[ref];
					html2canvas(dom,{
						useCORS: true,
						onrendered: function(canvas) {
					        var url = canvas.toDataURL();
					        $.ajax({
					          //url: window.protocol+'//api.zmiti.com/v2/share/base64_image/',
					          url:window.protocol+'//'+window.server+'.zmiti.com/v2/share/base64_image/',
					          type: 'post',
					          data: {
					            setcontents: url,
					            setwidth: dom.clientWidth,
					            setheight:dom.clientHeight
					          },
					          success: function(data) {
					          	//alert('data.getret =>'+data.getret)
					            if (data.getret === 0) {
					            	//s.deleteImg(dt.img);
					              var src = data.getimageurl;
					             	s.createImg = src;
	
									var url = window.location.href.split('#')[0];

									url = zmitiUtil.changeURLPar(url,'src',src);
									url = zmitiUtil.changeURLPar(url,'num',s.num);
									zmitiUtil.wxConfig('做新时代雷锋，今年我要做'+s.num+'件好事，立此存证',window.desc,url)
								       
					            }

					          }
					        })

					      },
					      width: dom.clientWidth,
					      height:dom.clientHeight
					})
				},100)
			}
			
		},
		mounted(){

			var {obserable} = this;

			obserable.on('titleShow',()=>{
				this.showTitle = true;
			});

			for(var i=1;i<101;i++){
				this.count.push({
					value:i,
					label:i
				})
			};
			this.count.push({
				value:'100+',
				label:'100+'
			});


			obserable.on('showIndexApp',(data)=>{
				this.show = true;
				if(data){
					this.createImg = data.src;
					this.src = data.src;
					this.num = data.num;

					var url = window.location.href.split('#')[0];

						url = zmitiUtil.changeURLPar(url,'src',this.src);
						url = zmitiUtil.changeURLPar(url,'num',this.num);
						zmitiUtil.wxConfig('做新时代雷锋，今年我要做'+this.num+'件好事，立此存证',window.desc,url)
				}
			})

			setTimeout(()=>{
				this.showHand = false;
			},2000)

		}
	}
</script>