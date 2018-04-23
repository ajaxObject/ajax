# 原生ajax以及跨域访问百度ajax

原生ajax:
dsdsada
```
function ajax(mJson){
	var xhr = window.XMLHttpRequest?new XMLHttpRequest():new ActiveXObject("Microsoft.XMLHTTP");
	var method = mJson.method || "get";
	var url = mJson.url;
	var asyn = mJson.asyn || true;
	var data = mJson.data;
	var success = mJson.success;

	if(method == "get" && data){
		url += "?"+data+"&"+Math.random();
	}
	xhr.open(method,url,asyn);
	//设置请求头
	xhr.setRequestHeader("content-type","application/x-www-form-urlencoded");
	if(method == "post" && data){
		xhr.send(data);
	}else{
		xhr.send();
	}
	xhr.onreadystatechange = function(){
		if(xhr.readyState==4 && xhr.status==200){
			if(success)success(xhr.responseText);
		}
	}
};
```

跨域访问百度 效果如下：

![](images/img.gif)

js demo code:

```
	var oTxt = document.getElementById("txt");
	var oList = document.getElementById("list");
	oTxt.onkeyup = function(){
		var val = this.value;
		var oS = document.createElement("script");
		oS.src = "https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd="+val+"&cb=andong";
		document.body.appendChild(oS);
		document.body.removeChild(oS);
	};
	function andong(mJson){
		var data = mJson.s;
		var str = "";
		for(var i=0;i<data.length;i++){
			str += "<li><a href='https://www.baidu.com/s?wd="+data[i]+"' target='_blank'>"+data[i]+"</a></li>";
		}
		oList.innerHTML = str;
	}
	oList.onmouseover = function(ev){
		ev = ev||event;
		var target = ev.target || srcElement;
		//console.log(target.tagName+"===="+ev.currentTarget.tagName);
		target.style.background = "#ccc";
	};
	oList.onmouseout = function(ev){
		ev = ev||event;
		var target = ev.target || srcElement;
		//console.log(target.tagName+"===="+ev.currentTarget.tagName);
		target.style.background = "";
	};
```

jquery demo code:

```
    $(function(){
        $("input").keyup(function(){
            $.ajax({
                type:"get",
                data:"wd="+$(this).val(),
                dataType:"jsonP",
                url:"http://suggestion.baidu.com/su",
                success:function(data){
                }
            });
        });
    });
    var baidu={
        sug:function(data){
            for(var i=0;i<10;i++){
                document.getElementsByTagName ("li")[i].setAttribute("class","aa");
                document.getElementsByTagName ("li")[i].innerText =data.s[i];
                if($("input").val().length>8||$("input").val().length==0){
                    $("li").removeAttr("class")
                }
            }

        }
    }
```

