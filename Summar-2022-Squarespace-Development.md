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

The Styling
```css
.variant-option{
  display: -webkit-box;
  display: flex;
  flex-wrap: wrap;
  -webkit-box-pack: flex-start;
  justify-content: flex-start;
  margin: 0 -5px;
  padding: 0;
  list-style: none;
}
ul.color-selector{
  position: relative;
  display: -webkit-box;
  display: flex;
  -webkit-box-pack: center;
  justify-content: center;
  margin-left: -40px;
}
li.color-selector-item {
  list-style: none;
}
.color-selector-button{
  box-sizing: border-box;
  position: relative;
  display: -webkit-box;
  display: flex;
  -webkit-box-pack: center;
  justify-content: center;
  -webkit-box-align: center;
  align-items: center;
  min-width: 50px;
  min-height: 50px;
  margin: 5px;
  padding: 0;
  background: none;
  background-color: transparent;
  background-position: 50%;
  background-repeat: no-repeat;
  background-size: cover;
  border-radius: 100%;
  border: 1px solid rgba(0, 0, 0, 0.3);
  box-shadow: none;
  color: currentColor;
  font-weight: 0;
  font: 0/0 a;
  line-height: 1;
  white-space: nowrap;
  outline: none;
  transition: background-color 0.25s, border 0.25s, box-shadow 0.25s, 
  color 0.25s;
}

.color-selector-button:hover,
.color-selector-button:focus,
.color-selector-button.is-active {
  border: 1px solid #000;
  box-shadow: none;
  background-color: transparent;
  color: currentColor;
}
```
