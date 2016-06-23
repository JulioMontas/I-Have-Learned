# Front-end with a Ajax call

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Front-end</title>
    <script  type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.2/jquery.min.js"></script>
  </head>
  <body>
    <script type="text/javascript">
      // $.get("http://localhost:3000/languages.json", function(data, status){
      //   console.log("data" + JSON.stringify(data) + "\n status:" + status);
      // });

      // $.post("http://localhost:3000/languages.json?name=Julio&price=12.00&active=false", function(data, status){
      //   console.log("data" + JSON.stringify(data) + "\n status:" + status);
      //   localStorage.setItem('product_id', data.product.id);
      // });

      $.ajax({
        url: "http://localhost:3000/languages/3",
        type: 'DELETE',
        success: function(result){
          console.log("data" + JSON.stringify(result) + "\n status:" + status);
        }
      });

      // $.ajax({
      //   url: "http://localhost:3000/languages/4/&name=Julio",
      //   type: 'PUT',
      //   success: function(result){
      //     console.log("data" + JSON.stringify(result) + "\n status:" + status);
      //   }
      // });

    </script>
  </body>
</html>
```

- Now we move to React.JS for the real Front-End
