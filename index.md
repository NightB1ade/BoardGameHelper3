<h1>Games List</h1>

<table id="GamesList">
<tbody>
	<tr>
	</tr>
</tbody>
</table>

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
+ "		<td>"
+ "			<a href='{{ 'Games/' | relative_url }}?bggid=" + $(v).attr("id") + "'>"
+ "				<div class='thumbnail'><img src='" + $(v).find("thumbnail").text() + "'></div>"
+ "				<div>" + $(v).find("name[type='primary']").attr("value") + "</div>"
+ "			</a>"
+ "		</td>";
				});
			}
		)
		.done(function(){
			$("#GamesList tbody tr").html(html);
		});
	});
});
</script>
