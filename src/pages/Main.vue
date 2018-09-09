<template>
	<div class="outSide">
		<div class="logo">Notes</div>
		<div 
		class="canvasOutside" 
		:style="canvasHeight" 
		@touchstart="handleTouchStart"
		@touchmove="handleTouchMove"
		@touchend="handleTouchEnd"
		>
        	<canvas class="canvasMain" 
        			ref="myCanvas" 
        			:width="canvasWidth" 
        			:height="canvassHeight">
        			阁下的浏览器版本不支持当前应用，请使用更加新的浏览器版本
        	</canvas>
    	</div>
    	<div class="option">
    		<select class="optionLeft" v-model="color">
    			<option value="white">White(Eraser)</option>
				<option value="black">Color(Black)</option>
				<option value="red">Color(Red)</option>
				<option value="blue">Color(Blue)</option>
				<option value="green">Color(Green)</option>
				<option value="purple">Color(Purple)</option>
			</select>
			<select class="optionRight" v-model="lineWidth">
				<option value="1">LineWidth(1)</option>
				<option value="2">LineWidth(2)</option>
				<option value="3" selected="selected">LineWidth(3)</option>
				<option value="4">LineWidth(4)</option>
				<option value="5">LineWidth(5)</option>
				<option value="6">LineWidth(6)</option>
				<option value="10">LineWidth(10)</option>
				<option value="15">LineWidth(15)</option>
			</select>
			<button @click="handleEraser" class="btnEraser">{{status}}</button>
			<button @click="handleClear" class="btnClear">Clear</button>
    	</div>
	</div>
</template>

<script>
export default{
	data(){
		return{
			status:'Eraser',
			colorObj:[{color:'white',show:'White(Eraser)'},{color:'black',show:'Color(Black)'},{color:'red',show:'Color(Red)'},{color:'Blue',show:'Color(Blue)'},{color:'Green',show:'Color(Green)'},{color:'Purple',show:'Color(Purple)'}],
			xs:'',
			ys:'',
			xe:'',
			ye:'',
			flag:0,
			color:'black',
			lineWidth:3,
			canvasDom:'',
			context:'',
			isMouseDown:false,
			adjust:'',
			saveStatus:[],
			canvassHeight:500,
			canvasWidth:300
		}
	},
	computed:{
		canvasHeight:function(){
			return 'height:' + 82 + 'vh'
		}
	},
	methods:{
		handleEraser(){
			if(this.status=='Eraser')
			{
				this.saveStatus[0] = this.color;
				this.saveStatus[1] = this.lineWidth;
				this.status = 'Pen';
				this.color = 'white';
				this.lineWidth = 15;

			}
			else
			{
				this.status = 'Eraser'
				this.color = this.saveStatus[0];
				this.lineWidth = this.saveStatus[1];
			}
		},
		handleClear(){ 
    		this.canvasDom.height=this.canvasDom.height;
		},
		handleTouchStart(e){
			this.isMouseDown=true;
            this.xs = e.touches[0].clientX;
            this.ys = e.touches[0].clientY;
            this.context.lineWidth=this.lineWidth;
            this.context.strokeStyle=this.color;
            this.adjust = this.canvasDom.getBoundingClientRect();
            e.preventDefault && e.preventDefault();
   			e.stopPropagation && e.stopPropagation();
		},
		handleTouchMove(e){
			if(this.isMouseDown){
                this.xe = e.touches[0].clientX;
                this.ye = e.touches[0].clientY;
                self = this;
                this.drowline(self.xs,self.ys,self.xe,self.ye);
                this.flag++;
            }
		},
		handleTouchEnd(){
			this.isMouseDown=false;
            this.flag=0;
		},
        drowline(num1,num2,num3,num4){
			num1 = (num1 - this.adjust.left) * (this.canvasDom.width / this.adjust.width);
			num2 = (num2 - this.adjust.top) * (this.canvasDom.height / this.adjust.height);
			num3 = (num3 - this.adjust.left) * (this.canvasDom.width / this.adjust.width);
			num4 = (num4 - this.adjust.top) * (this.canvasDom.height / this.adjust.height);
			//console.log('初始X'+num1);
            //console.log('初始Y'+num2);
            //console.log('移动X'+num3);
           	//console.log('移动Y'+num4);
            //开启新的路径
            if(this.flag)
            this.context.beginPath();
            //移动画笔的初始位置
            this.context.moveTo(num1,num2);
            //移动画笔的结束位置
            this.context.lineTo(num3,num4);
            //开始绘制
            this.context.stroke();

            if(this.flag!=0){
                this.xs=this.xe;
                this.ys=this.ye;
            }
        }


	},
	mounted(){
		this.canvasDom = this.$refs.myCanvas;
		this.context = this.canvasDom.getContext("2d");
		this.canvasWidth = document.documentElement.clientWidth;
		this.canvassHeight = document.documentElement.clientHeight;

	}


}
</script>

<style lang="stylus">
	.outSide
		padding .2rem .2rem .2rem .2rem
		text-align center
		.logo
			font-size .8rem
		.canvasOutside
			position relative
			top .12rem
			border 1px solid black
			overflow hidden
		.option
			height .1rem
			position relative
			bottom -.25rem
			width 100%
			display flex
			.optionLeft
				flex .2
				height .4rem
			.optionRight
				flex .2
				height .4rem
			.btnClear
				flex .3
				height .4rem
				background-color red
				color white
			.btnEraser
				flex .3
				height .4rem
				background-color green
				color white
				
	
			


	
</style>