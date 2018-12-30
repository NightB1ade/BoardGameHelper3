<h1>Games List</h1>

<div id="GamesList">
</div>




<script>
$(document).ready(function(){
	var BGGIDList = "";
	var html = "";

	$.get(
		"{{ 'Games/GamesList.xml' | relative_url }}"
		,function(data){
			BGGIDList = $(data).find("Games Game").map(function(){
				return $(this).attr("id");
			})
			.get()
			.join();
		}
	)
	.done(function(){
		$.get(
			"{{ site.bggapi-thing }}" + BGGIDList
			,function(data){
				var item = $(data).find("items item");

				item.sort(function(a,b){
					return ($(a).find("name[type='primary']").attr("value") > $(b).find("name[type='primary']").attr("value")) ? 1 : 0;
				});

				item.each(function(i,v){
html += ""
+ "	<div>"
+ "		<a href='Games/?bggid=" + $(v).attr("id") + "'>"
+ "			<span class='thumbnail'><img src='" + $(v).find("thumbnail").text() + "'></span>"
+ "			<span>" + $(v).find("name[type='primary']").attr("value") + "</span>"
+ "		</a>"
+ "	</div>";
				});
			}
		)
		.done(function(){
			$("#GamesList").html(html);
		});
	});
});
</script>
