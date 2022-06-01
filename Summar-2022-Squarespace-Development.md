# Squarespace Development


• https://medium.com/@jdbowen1994/first-time-squarespace-code-injection-6eda0993fb66
• https://crane-cinnamon-w8mr.squarespace.com/store-1/p/style-01-hs2wj-rec4m-3dpak-dndkp

one
```html
<!-- Start color boxes  -->
<script>
//Hides both dropdown boxes
$("div.variant-select-wrapper").hide();

// Then adding a <ul> tag, using the correct selectors here is slightly complex  
$("div.variant-select-wrapper:has(select[data-variant-option-name='Color'])").after("<ul class='color-selector'>");
  
//for every option in the select tag except the first
$("div.variant-select-wrapper:has(select[data-variant-option-name='Color']) option:not(:first)")
.each(function () {
		var color = $(this).val(); //store the color
		$(
			"<li class='color-selector-item'> <button class='color-selector-button' type='button' title='" +
				color +
				"' style='background-color: " +
				color +
				" !important;'>" +
				color +
				"</button></li>"
		).appendTo("ul.color-selector");
	});
	//after the loop close the <ul> tag
	$("li.color-selector-item:last").after("</ul>");
  
  
$("button.color-selector-button").click(function () {
  //Chnage the text on the screen
  $("div.variant-option-title:contains('Color')").html(
	"Color: " + $(this).text()
  );
});  
  
document.querySelector("select[data-variant-option-name='Color']").dispatchEvent(new Event("change"));

$(document).ready(function () {
	let myColour = "Yellow";
	let mySize = "M";
  
	$(".variant-option-title").click(function () {
		$("select[data-variant-option-name=Color]").val(myColour);
		document.querySelector("select[data-variant-option-name='Color']").dispatchEvent(new Event("change"));
		$("select[data-variant-option-name=Size]").val(mySize);
    document.querySelector("select[data-variant-option-name='Size']").dispatchEvent(new Event("change"));
	});
});  
  
</script>
```

Two: Working
```html
<!-- Start Plugin -->
<script>
$(document).ready(function(){$("div.variant-select-wrapper:contains('Color')").addClass("color-wrapper"),$("div.variant-select-wrapper:contains('Size')").addClass("size-wrapper"),$("div.variant-select-wrapper").hide(),$("div.variant-select-wrapper:has(select[data-variant-option-name='Color'])").after("<ul class='color-selector'>"),$("div.variant-select-wrapper:has(select[data-variant-option-name='Color'])").after("<div id='color-test'></div>"),$("div.variant-select-wrapper:has(select[data-variant-option-name='Color']) option:not(:first)").each(function(){var t=$(this).val(),e=document.getElementById("color-test");e.style.backgroundColor="",e.style.backgroundColor=t;var a={exists:function(t){return t&&""!==t.style.backgroundColor}};a.exists(e,t)||alert("Doesnt exist - "+t),$("<li class='color-selector-item'> <button class='color-selector-button' type='button' title='"+t+"' style='background-color: "+t+" !important;'>"+t+"</button></li>").appendTo("ul.color-selector")}),$("li.color-selector-item:last").after("</ul>"),$("button.color-selector-button").click(function(){$("div.variant-option-title:contains('Color')").html("Color: "+$(this).text()),$("div.variant-select-wrapper.color-wrapper").attr("data-text",$(this).text()).change(),$("select[data-variant-option-name=Color]").val($(this).text()),document.querySelector("select[data-variant-option-name='Color']").dispatchEvent(new Event("change"))}),$("div.variant-select-wrapper:has(select[data-variant-option-name='Size'])").after("<ul class='size-selector'>"),$("div.variant-select-wrapper:has(select[data-variant-option-name='Size']) option:not(:first)").each(function(){$("<li class='size-selector-item'> <button class='size-selector-button' type='button' title='"+$(this).val()+"'>"+$(this).val()+"</button></li>").appendTo("ul.size-selector")}),$("li.size-selector-item:last").after("</ul>"),$("button.size-selector-button").click(function(){$("div.variant-option-title:contains('Size')").html("Size: "+$(this).text()),$("div.variant-select-wrapper.size-wrapper").attr("data-text",$(this).text()).change(),$("select[data-variant-option-name=Size]").val($(this).text()),document.querySelector("select[data-variant-option-name='Size']").dispatchEvent(new Event("change"))})});
</script>
<!-- Ends Plugin -->
```
