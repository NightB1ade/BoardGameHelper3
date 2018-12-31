---
title: Game
layout: game
---

<span>
	<img id="thumbnail">
	<table id="Parameters">
		<tr id="NoPlayers" class="hidden">
			<td>No. Players</td>
			<td class="input"></td>
		</tr>
		<tr>
			<td colspan="2">
				<button>Setup</button>
				<button onclick="DisplayEndGame()">End Game</Button>
			</td>
		</tr>
	</table>
</span>

<div id="GameInfo">
</div>




<script>
var gamedata;
var url = new URL(window.location.href);
var BGGID = url.searchParams.get("bggid");




function DisplayEndGame() {
	var html = "<h1>End Game</h1>";
	var endgamedata = $(gamedata).find("Base EndGame");

	// End Game Triggers
	if ($(endgamedata).find("Trigger Text Item").first().text() != "") {
		var text = $(endgamedata).find("Trigger Text Item");
		html += "<h2>Trigger</h2><ul>";
		$(text).each(function(){
			html += "<li>" + $(this).text() + "</li>";
		});
		html += "</ul>";
	}

	// End Game Scoring
	if ($(endgamedata).find("Scoring Text Item").text() != "") {
		var text = $(endgamedata).find("Scoring Text Item");
		html += "<h2>Scoring</h2><ul>";
		$(endgamedata).find("Scoring Text Item").each(function(){
			html += "<li>" + $(this).text() + "</li>";
		});
		html += "</ul>";
	}

	$("#GameInfo").html(html);
}




$(document).ready(function(){
	var url = new URL(window.location.href);
	var BGGID = url.searchParams.get("bggid");

	//Get BGGAPI Data
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

	// Get Internal Data
	$.get(
		"{{ /Games/ | relative_url }}" + BGGID + ".xml"
		,function(data){
			gamedata = $(data).find("Game");

			// Unhide player number chooser if appropriate
			var MinPlayers = $(gamedata).find("Base Players MinPlayers").text();
			var MaxPlayers = $(gamedata).find("Base Players MaxPlayers").text();
			if (MinPlayers != MaxPlayers) {
				$("#Parameters #NoPlayers").css("visibility","visible");
				$("#Parameters #NoPlayers td.input").html("INPUT");
			}
		}
	);
});
</script>
