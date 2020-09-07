<template>

	<div id="app" style="display: block; height: 1200px">
		<div style="float:right">
			<button class="login-btn">Start</button>
		</div>
		<div style=" margin: auto;
  width: 50%;
  padding: 10px; 
  padding-bottom: 0px;
    width: 300px;
    text-align: left;
	margin-bottom: -20px;
  ">
			<b>Total Scanned: {{totalscan}}</b>
			<br/>
			<b>BC ID: {{bcid}}</b>
			<br/>
			<b>BC Code: {{bccode}}</b>
			<br/>
			<b>Account: {{wateraccount}}</b>
			<br/>
			<b>Invoice Number: {{invoicenumber}}</b>
		</div>
		
		<br/>
		
		<div>
		
		<input type="text" v-model="totalscan" id="totalscan" style="display:none"/>
		<input type="text" v-model="bcid" id="bcid" style="display:none"/>
		<input type="text" v-model="bccode" id="bccode" style="display:none"/>
		<input type="text" v-model="wateraccount" id="wateraccount" style="display:none"/>
		<input type="text" v-model="invoicenumber" id="invoicenumber" style="display:none"/>
		
		<Progress 
		 :transitionDuration="rpatime"
		  :radius="42"
		  :strokeWidth="10"
		  :value="rpapercent" style="margin:0px; display: block"
		>
		  <template v-slot:footer>
			<b>Data Capture</b>
		  </template>
		</Progress>
		<input type="range" max="100000000000" v-model="rpatime" id="rpatime" style="display:none"/>
		<input type="text" v-model="rpapercent" id="rpapercent" style="display:none"/>
		
		<Progress 
			:transitionDuration="ocrtime"
			  :radius="42"
			  :strokeWidth="10"
			  :value="ocrpercent" style="margin:0px; display: block"
			>
		  <template v-slot:footer>
			<b>OCR</b>
		  </template>
		</Progress>
		<input type="range" max="100000000000" v-model="ocrtime" id="ocrtime" style="display:none"/>
		<input type="text" v-model="ocrpercent" id="ocrpercent" style="display:none"/>
		

		<Progress 
		  :transitionDuration="edmstime"
		  :radius="42"
		  :strokeWidth="10"
		  :value="edmspercent" style="margin:0px; display: block"
			>
		  <template v-slot:footer>
			<b>EDMS</b>
		  </template>
		</Progress>
		<input type="range" max="100000000000" v-model="edmstime" id="edmstime" style="display:none"/>
		<input type="text" v-model="edmspercent" id="edmspercent" style="display:none"/>
		

		<Progress
		  :transitionDuration="smstime"
		  :radius="42"
		  :strokeWidth="10"
		  :value="smspercent" style="margin:0px; display: block"
		>
		  <template v-slot:footer>
			<b>Core</b>
		  </template>
		</Progress>
		<input type="range" max="100000000000" v-model="smstime" id="smstime" style="display:none"/>
		<input type="text" v-model="smspercent" id="smspercent" style="display:none"/>


		<Progress
		   strokeColor="#FF00AA" 
		  :transitionDuration="bankingtime"
		  :radius="42"
		  :strokeWidth="10"
		  :value="bankingpercent" style="margin:0px; display: block"
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
var messageFile = 'monitor.message';
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
				totalscan: '',
				bcid: '',
				bccode: '',
				wateraccount: '',
				invoicenumber: ''
				
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
		if(isNaN(value)){
			ele.value = value;
		}
		else{
			ele.value = Math.floor(value);
		}
		ele.dispatchEvent(event);
	}

	var blur = 0.15;
	var rpa_time = 1000 * 45;  //1000 * 120
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
		//console.log('go');
		var dd = new Date();
		var now = dd.getTime();
		
		var preAccount = account;
		readMessageFile(messageFile);
		var rpa_finished = false;
		if(preAccount!=account){
			if(previous_status == 'succeed'){
				console.log('succeed');
				rpa_end = now - 1;
				rpa_finished = true;
			}else{
				var blur_2 = (100 + getRndInteger(-blur*100, blur*100))/100;
				rpa_time_2 = rpa_time * blur_2;
				rpa_end = now + rpa_time_2;
				setElement("rpatime", 500);
				setElement("rpapercent", 1);
				setTimeout(function(){
					setElement("rpatime", rpa_time_2);
					setElement("rpapercent", 100);
				}, 1000);
				
				setElement("totalscan", counter);
				setElement("bcid", bcid);
				setElement("bccode", bccode);
				setElement("wateraccount", account);
				setElement("invoicenumber", invoicenumber);
			}
		}
		
		
		if (rpa_end == 0){
			var blur_2 = (100 + getRndInteger(-blur*100, blur*100))/100;
			rpa_time_2 = rpa_time * blur_2;
			rpa_end = now + rpa_time_2;
			setElement("rpatime", rpa_time_2);
			setElement("rpapercent", 100);
		}else{
			//console.log(now);
			//console.log(rpa_end);
			if (now > rpa_end && rpa_finished){
				console.log('new rpa');
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
				
				setTimeout(function(){
					setElement("rpatime", 500);
					setElement("rpapercent", 99);
				}, 500);
				
				setTimeout(function(){
					setElement("rpapercent", 1);
				}, 1500);
				
				setTimeout(function(){
					setElement("rpatime", rpa_time_2);
					setElement("rpapercent", 100);				
				}, 2000);
				
				setElement("ocrtime", 500);
				setElement("ocrpercent", 1);
				setTimeout(function(){
					setElement("ocrtime", ocr_time_2);
					setElement("ocrpercent", 100);
				}, 1000);
				
				setTimeout(function(){
					setElement("edmstime", 500);
					setElement("edmspercent", 1);
				}, 1000);
				setTimeout(function(){
					setElement("smstime", 500);
					setElement("smspercent", 1);
				}, 1000);
				setTimeout(function(){
					setElement("bankingtime", 500);
					setElement("bankingpercent", 0);
				}, 1000);
				
				setElement("totalscan", counter);
				setElement("bcid", bcid);
				setElement("bccode", bccode);
				setElement("wateraccount", account);
				setElement("invoicenumber", invoicenumber);
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
				setElement("bankingpercent", 0);
				setTimeout(function(){
					setElement("bankingtime", bankingtime_2);
					setElement("bankingpercent", 0);
				}, 1000);
			}
			
		}
	}
	
	var t = setInterval(go, 1000);
	
	//setTimeout(function(){ go(); }, 1000);
	
	/*
	document.addEventListener('DOMContentLoaded', function() {
		console.log(1);
		go();
	}, false);
	*/

	var counter;
	var bcid;
	var bccode;
	var account;
	var invoicenumber;
	var previous_status;
	function readMessageFile(file) {
		var allText = readTextFile(file);
		var allTextLines = allText.split(/\r\n|\n/);
		for (var i=0; i<allTextLines.length; i++) {
			var data = allTextLines[i].split('=');
			if (data.length >= 2) {
				if(data[0] == 'counter'){
					counter = data[1];
				}else if (data[0] == 'bcid'){
					bcid = data[1];
				}else if (data[0] == 'bccode'){
					bccode = data[1];
				}else if (data[0] == 'account'){
					account = data[1];
				}else if (data[0] == 'invoicenumber'){
					invoicenumber = data[1];
				}else if (data[0] == 'previous'){
					previous_status = data[1];
				}
			}
		}
	}
	function readTextFile(file)
	{
		var rawFile = new XMLHttpRequest();
		var allText;
		rawFile.open("GET", file, false);
		rawFile.onreadystatechange = function ()
		{
			if(rawFile.readyState === 4)
			{
				if(rawFile.status === 200 || rawFile.status == 0)
				{
					allText = rawFile.responseText;
				}
			}
		}
		rawFile.send(null);
		return(allText);
	}
	
	function processData(allText) {
		var allTextLines = allText.split(/\r\n|\n/);
		var headers = allTextLines[0].split(',');
		var lines = [];

		for (var i=1; i<allTextLines.length; i++) {
			var data = allTextLines[i].split(',');
			if (data.length == headers.length) {
				var obj = new Object();
				for (var j=0; j<headers.length; j++) {
					obj[headers[j]] = data[j];
				}
				lines.push(obj);
			}
		}
		return lines;
	}



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






