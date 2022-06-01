# Squarespace Development

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
