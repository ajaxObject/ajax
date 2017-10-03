# 原生ajax以及跨域访问百度ajax

原生ajax:

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

