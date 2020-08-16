<template>

	<div id="app" style="display: block">
		
		<div style=" margin: auto;
  width: 50%;
  padding: 100px; 
  padding-top: 100px;
  
    width: 200px;
    text-align: left;
  
  ">
			<b>BC ID: 686</b>
			<br/>
			<b>BC Code: 209972</b>
			<br/>
			<b>Account: {{Account}}</b>
		</div>
		
		<br/>
		
		<div>
		<Progress 
		 :transitionDuration="rpatime"
		  :radius="50"
		  :strokeWidth="10"
		  :value="rpapercent" style="margin:20px"
		>
		  <template v-slot:footer>
			<b>RPA Scanning</b>
		  </template>
		</Progress>
		<input type="range" max="100000000000" v-model="rpatime" id="rpatime" style="display:none"/>
		<input type="text" v-model="rpapercent" id="rpapercent" style="display:none"/>
		
		<Progress 
			strokeColor="#FF00AA" 
			:transitionDuration="ocrtime"
			  :radius="50"
			  :strokeWidth="10"
			  :value="ocrpercent" style="margin:20px"
			>
		  <template v-slot:footer>
			<b>OCR</b>
		  </template>
		</Progress>
		<input type="range" max="100000000000" v-model="ocrtime" id="ocrtime" style="display:none"/>
		<input type="text" v-model="ocrpercent" id="ocrpercent" style="display:none"/>
		

		<Progress 
		  :transitionDuration="edmstime"
		  :radius="50"
		  :strokeWidth="10"
		  :value="edmspercent" style="margin:20px"
			>
		  <template v-slot:footer>
			<b>EDMS</b>
		  </template>
		</Progress>
		<input type="range" max="100000000000" v-model="edmstime" id="edmstime" style="display:none"/>
		<input type="text" v-model="edmspercent" id="edmspercent" style="display:none"/>
		

		<Progress
		  :transitionDuration="smstime"
		  :radius="50"
		  :strokeWidth="10"
		  :value="smspercent" style="margin:20px"
		>
		  <template v-slot:footer>
			<b>SMS</b>
		  </template>
		</Progress>
		<input type="range" max="100000000000" v-model="smstime" id="smstime" style="display:none"/>
		<input type="text" v-model="smspercent" id="smspercent" style="display:none"/>


		<Progress
		  :transitionDuration="bankingtime"
		  :radius="50"
		  :strokeWidth="10"
		  :value="bankingpercent" style="margin:20px"
		>
		  <template v-slot:footer>
			<b>Banking</b>
		  </template>
		</Progress>
		<input type="range" max="100000000000" v-model="bankingtime" id="bankingtime" style="display:none"/>
		<input type="text" v-model="bankingpercent" id="bankingpercent" style="display:none"/>
		

		<div style="display:none">
			<Progress :transitionDuration="value2" :radius="55" :strokeWidth="10" :value="value3">
			  <!--<div class="content">hello</div>-->
			  <template v-slot:footer>
				<b>More Precise</b>
			  </template>
			</Progress>
			<input type="range" max="10000" v-model="value2" id="test2" />
			<input type="text" v-model="value3" id="value3" />
		</div>
	
		</div>
	
	
	</div>
</template>






<script>

import Progress from 'easy-circular-progress'
//Vue.use(ProgressBar)

    export default {
        components: {
            Progress
        },

        data() {
            return {
				value2: 100000000000,
				value3: 0,
				value_1: 0,
				rpatime: 100000000000,
				rpapercent: 0,
				ocrtime: 100000000000,
				ocrpercent: 0,
				edmstime: 100000000000,
				edmspercent: 0,
				smstime: 100000000000,
				smspercent: 0,
				bankingtime: 100000000000,
				bankingpercent: 0,
				Account: '5422818-01'
            }
        }
    }
	
	var p = 0;
	function setP(){
				p += 1;
				if(p==101){
					p = 0;
				}
				var ele = document.getElementById("test2")
				ele.value = p;
				var event = new Event('input', {
					bubbles: true,
					cancelable: true,
				});
				ele.dispatchEvent(event);
	}
	//var t = setInterval(setP, 1000);
	//clearInterval(t);

	function getRndInteger(min, max) {
	  return Math.floor(Math.random() * (max - min)) + min;
	}
	
	function sleep(ms) {
	  return new Promise(resolve => setTimeout(resolve, ms));
	}
	
	//string, int
	function setElement(element, value){
		var event= new Event('input', {
			bubbles: true,
			cancelable: true,
		});
		var ele = document.getElementById(element)
		ele.value = Math.floor(value);
		ele.dispatchEvent(event);
	}



	var blur = 0.15;
	var rpa_time = 1000 * 120;  //1000 * 120
	var ocr_time = 1000 * 10;
	var edms_time = 1000 * 8;
	var sms_time = 1000 * 15;
	var banking_time = 1000 * 6;
	
	var rpa_time_2;
	var ocr_time_2;
	var edmstime_2;
	var smstime_2;
	var bankingtime_2;	
	
	var rpa_end = 0;
	var ocr_end = 9007199254740992;
	var edms_end = 9007199254740992;
	var sms_end = 9007199254740992;
	var banking_end = 9007199254740992;
	
	var edms_started = false;
	var sms_started = false;
	var banking_started = false;
	
	function go(){
		console.log('go');
		var dd = new Date();
		var now = dd.getTime();
		
		if (rpa_end == 0){
			var blur_2 = (100 + getRndInteger(-blur*100, blur*100))/100;
			rpa_time_2 = rpa_time * blur_2;
			rpa_end = now + rpa_time_2;
			setElement("rpatime", rpa_time_2);
			setElement("rpapercent", 100);
		}else{
			console.log(now);
			console.log(rpa_end);
			if (now > rpa_end){
				var blur_2 = (100 + getRndInteger(-blur*100, blur*100))/100;
				rpa_time_2 = rpa_time * blur_2;
				rpa_end = now + rpa_time_2;
				
				blur_2 = (100 + getRndInteger(-blur*100, blur*100))/100;
				ocr_time_2 = ocr_time * blur_2;
				ocr_end = now + ocr_time_2;
				
				blur_2 = (100 + getRndInteger(-blur*100, blur*100))/100;
				edmstime_2 = edms_time * blur_2;
				edms_end = ocr_end + edmstime_2;
				
				blur_2 = (100 + getRndInteger(-blur*100, blur*100))/100;
				smstime_2 = sms_time * blur_2;
				sms_end = edms_end + smstime_2;
				
				blur_2 = (100 + getRndInteger(-blur*100, blur*100))/100;
				bankingtime_2 = banking_time * blur_2;
				banking_end = sms_end + bankingtime_2;
				
				edms_started = false;
				sms_started = false;
				banking_started = false;
				
				setElement("ocrtime", 500);
				setElement("ocrpercent", 1);
				setTimeout(function(){
					setElement("ocrtime", ocr_time_2);
					setElement("ocrpercent", 100);
				}, 1000);
				
				setElement("rpatime", 500);
				setElement("rpapercent", 1);
				setTimeout(function(){
					setElement("rpatime", rpa_time_2);
					setElement("rpapercent", 100);
				}, 1000);
				setTimeout(function(){
					setElement("edmstime", 500);
					setElement("edmspercent", 0);
				}, 1000);
				setTimeout(function(){
					setElement("smstime", 500);
					setElement("smspercent", 0);
				}, 1000);
				setTimeout(function(){
					setElement("bankingtime", 500);
					setElement("bankingpercent", 0);
				}, 1000);
				
			}
			if (!edms_started && now > ocr_end){
				edms_started = true;
				setElement("edmstime", 500);
				setElement("edmspercent", 1);
				setTimeout(function(){
					setElement("edmstime", edmstime_2);
					setElement("edmspercent", 100);
				}, 1000);
			}
			if (!sms_started && now > edms_end){
				sms_started = true;
				setElement("smstime", 500);
				setElement("smspercent", 1);
				setTimeout(function(){
					setElement("smstime", smstime_2);
					setElement("smspercent", 100);
				}, 1000);
			}
			if (!banking_started && now > sms_end){
				banking_started = true;
				setElement("bankingtime", 500);
				setElement("bankingpercent", 1);
				setTimeout(function(){
					setElement("bankingtime", bankingtime_2);
					setElement("bankingpercent", 50);
				}, 1000);
			}
			
		}
	}
	
	var t = setInterval(go, 4000);
	
	//setTimeout(function(){ go(); }, 1000);
	
	/*
	document.addEventListener('DOMContentLoaded', function() {
		console.log(1);
		go();
	}, false);
	*/




</script>


<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  height: 100vh;
  color: #fff;
  background: #3e423a;
  display: flex;
  justify-content: center;
  align-items: center;
}
body {
  margin: 0;
  padding: 0;
}
</style>






