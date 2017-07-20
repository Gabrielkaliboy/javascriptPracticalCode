---
title: javascript实用代码段
date: 2017-07-17 15:30:40
categories: 前端
tags: [javascript]
---
<Excerpt in index | 首页摘要> 
javascript使用代码段
<!-- more -->
<The rest of contents | 余下全文>

-----
#### 1.随机颜色
```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
     <button id="btn1">调用第一种</button>
     <button id="bnt2">调用第二种</button>
     <button id="btn3">调用第三种</button>
     <script>
         var btn1=document.getElementById('btn1');
         btn1.onclick=function(){
             document.body.style.background=bg1()
         };
         var btn2=document.getElementById('bnt2');
         btn2.onclick=function(){
             document.body.style.background=bg2();
         };
         var btn3=document.getElementById('btn3');
         btn3.onclick=function(){
             document.body.style.background=bg3();
         };
         function bg1(){
             return '#'+Math.floor(Math.random()*256).toString(10);
         }
         function bg2(){
             return '#'+Math.floor(Math.random()*0xffffff).toString(16);
         }
         function bg3(){
             var r=Math.floor(Math.random()*256);
             var g=Math.floor(Math.random()*256);
             var b=Math.floor(Math.random()*256);
             return "rgb("+r+','+g+','+b+")";//所有方法的拼接都可以用ES6新特性`其他字符串{$变量名}`替换
         }
     </script>
</body>
</html>
```
#### 2.随机验证码
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<style>
		#code{  
               font-family:Arial;  
               font-style:italic;  
               font-weight:bold;  
               border:0;  
               letter-spacing:2px;  
               color:blue;  
           }
	</style>
	<title>javascript随机验证码</title>
</head>
<body>
	<div>  
	    <input type = "text" id = "input" value="" />  
	    <input type = "button" id="code" onclick="createCode()"/>  
	    <input type = "button" value = "验证" onclick = "validate()"/>  
	</div> 
</body>
<script>
	//设置一个全局的变量，便于保存验证码
	    var code;
	    function createCode(){
	        //首先默认code为空字符串
	        code = '';
	        //设置长度，这里看需求，我这里设置了4
	        var codeLength = 4;
	        var codeV = document.getElementById('code');
	        //设置随机字符
	        var random = new Array(0,1,2,3,4,5,6,7,8,9,'A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R', 'S','T','U','V','W','X','Y','Z');
	        //循环codeLength 我设置的4就是循环4次
	        for(var i = 0; i < codeLength; i++){
	            //设置随机数范围,这设置为0 ~ 36
	             var index = Math.floor(Math.random()*36);
	             //字符串拼接 将每次随机的字符 进行拼接
	             code += random[index]; 
	        }
	        //将拼接好的字符串赋值给展示的Value
	        codeV.value = code;
	    }

	    //下面就是判断是否== 的代码，无需解释
	    function validate(){
	        var oValue = document.getElementById('input').value.toUpperCase();
	        if(oValue ==0){
	            alert('请输入验证码');
	        }else if(oValue != code){
	            alert('验证码不正确，请重新输入');
	            oValue = ' ';
	            createCode();
	        }else{
	            window.open('http://www.baidu.com','_self');
	        }
	    }

	    //设置此处的原因是每次进入界面展示一个随机的验证码，不设置则为空
	    window.onload = function (){

	        createCode();
	    }
</script>
</html>
```

#### 3.数组去重（原创）
思路：
- 新建一个空数组
- 使用indexOf方法在空数组中寻找已知数组的每个元素，如果不存在的话会返回-1

- 返回-1,就把当前i指向的值存入空数组，最后返回空数组就行了
```javascript
var dataArray=[1,2,3,1,2,3];
function deleteRepeat(dataArray){
    var newArray=[];
    for(var i=0;i<dataArray.length;i++){
        if(newArray.indexOf(dataArray[i]) == -1){
            newArray.push(dataArray[i]);
        }
    }
    return newArray;
};
console.log(deleteRepeat(dataArray));
```
#### 4.数组去重（借鉴）
思路：
- 把当前数组里面的第一个值存入result数组

- 和第一个一样，循环遍历原数组里面的每个值，如果在result数组里面能够匹配到（二者相等），就说明当前i指向的那个值在result里面已经存在，此时将通信员repeat赋值为true，并且跳出当前循环，去拿下一个i。拿到下一个i,repeat再次被更改为false.

- 如果没能进入m所在的if循环，则repeat一致为false，这时候就把当前i指向的那个值压入result

- 随后返回result
```javascript
var dataArray=[1,2,3,1,2,3,"dd","cc","dd","cc"];
Array.prototype.deleteRepeat=function(){
    //这里的this指向的就是当前的数组，比如dataArray
    //result就是去重以后的数组
    //注意这里的写法，直接将result写为了数组
    var result=[this[0]];
    for(var i=0;i<this.length;i++){
        var repeat=false;
        for(var m=0;m<result.length;m++){
            if(this[i]==result[m]){
                //能进入这个循环，说明当前的this[i]在resutl里面有，也就是重复
                //将repeat设为true,不让其进入下面的if循环
                repeat=true;
                break;
            }
        }
        if(!repeat){
            //进入这个的，也就是没有重复，没有重复就把当前的this[i]扔进result
            result.push(this[i]);
        }
    }
    return result;
}
console.log(dataArray.deleteRepeat());
```
#### 5.数组去重（借鉴）
思路：
- 先将当前的数组进行排序，保证相同的值都是在一起的

- 最重要的一句`this[i] !==result[result.length-1]`,result里面最后一个值是最新的值（push进入的吗），由于this已经排序过了，所以直接筛选掉了比最后一个result值小的值。
```javascript
Array.prototype.deleteRepeat=function(){
    this.sort();
    var result=[this[0]];
    for(i=1;i<this.length;i++){
        if(this[i] !==result[result.length-1]){
            result.push(this[i]);
        }
    }
    return result;
}
var dataArray=[1,2,3,1,2,3,"dd","cc","dd","cc"];
console.log(dataArray.deleteRepeat());
```

#### 禁止表单按回车触发提交事件
在HTML页里面由于使用了form，常常需要禁用enter提交表单。因为内容页或者母版页自身有如果有type="submit"的button,当textbox聚焦时，按下enter都会触发表单的默认提交（不论是IE还是firefox），于是需要在onkeydown中监听用户的按键。实际测试，IE8中导致表单提交的不确定因素太多，点击表单的table中的td都会触发表单提交，而firefox则不会；于是在ie和ff中禁用表单提交需要不同的思路。

- 对于IE:
只有当事件源是TEXTAREA时才return true，允许默认动作；其他元素全部return false，禁止表单提交和任何响应。

- 对于firefox:
只有当事件源是INPUT时才return false禁止表单默认动作；而其他元素则return true允许默认动作，比如textarea的多行输入。

代码段
```javascript
//禁用Enter键表单自动提交    
document.onkeydown = function(event) {    
   var target, code, tag;    
   if (!event) {    
       event = window.event; //针对ie浏览器    
       target = event.srcElement;    
       code = event.keyCode;    
       if (code == 13) {    
           tag = target.tagName;  
           //判断是不是textarea，如果是返回true，不是则禁用  
           if (tag == "TEXTAREA") { return true; }    
           else { return false; }    
       }    
   }    
   else {    
       target = event.target; //针对遵循w3c标准的浏览器，如Firefox    
       code = event.keyCode;    
       if (code == 13) {    
           tag = target.tagName;    
           if (tag == "INPUT") { return false; }    
           else { return true; }    
       }    
   }    
};  
```