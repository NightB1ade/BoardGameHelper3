# Games List

<table id="GamesList">
<thead>
	<tr>
		<th colspan="2">Game</th>
	</tr>
</thead>
<tbody>
</tbody>
</table>

<script>
$(document).ready(function(){
	var BGGIDList = "";
	var html = "";

	$.get(
		"{{ Games/GamesList.xml | relative_url }}"
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
+ "	<tr>"
+ "		<td><img src='" + $(v).find("thumbnail").text() + "'></td>"
+ "		<td>" + $(v).find("name[type='primary']").attr("value") + "</td>"
+ "	</tr>";
				});
			}
		)
		.done(function(){
			$("#GamesList tbody").html(html);
		});
	});
});
</script>
