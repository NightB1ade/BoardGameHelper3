---
title: Game
---

<img id="thumbnail">


<script>
$(document).ready(function(){
	var url = new URL(window.location.href);
	var BGGID = url.searchParams.get("bggid");

	$.get(
		"{{ site.bggapi-thing }}" + BGGID
		,function(data){
			var item = $(data).find("items item");

			var name = $(item).find("name[type='primary']").attr("value");
			$("head title").html(name + " - {{ site.title }}");
			$("section#title h1").html(name);

			var img = $(item).find("thumbnail").text();
			$("img#thumbnail").attr("src",img);
		}
	);
});
</script>
