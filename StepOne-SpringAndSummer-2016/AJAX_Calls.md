# Front-end with a Ajax call

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Front-end</title>
    <script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.2/jquery.min.js"></script>
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/handlebars.js/4.0.5/handlebars.min.js"></script>
  </head>
  <body>
    <h1>AJAX Testing</h1>
  </body>
  <script type="text/javascript">

    // Find()
    // $.get("http://localhost:1337/user/", function(data, status){
    //   console.log("data" + JSON.stringify(data) + "\n status:" + status);
    // });

    // Find(id)
    // $.get("http://localhost:1337/user/2", function(data, status){
    //   console.log("data" + JSON.stringify(data) + "\n status:" + status);
    // });

    // Create()
    $.post("http://localhost:1337/user/create?name=Jasfasose", function(data, status){
      console.log("data" + JSON.stringify(data) + "\n status:" + status);
    });

    // Update(id)
    // $.ajax({
    //   url: "http://localhost:1337/user/2/?firstName=Cantinfla",
    //   type: 'PUT',
    //   success: function(result){
    //     console.log("data " + JSON.stringify(result));
    //   }
    // });

    // Destroy(id)
    // $.ajax({
    //   url: "http://localhost:1337/user/7/",
    //   type: 'DELETE',
    //   success: function(result){
    //     console.log("data" + JSON.stringify(result));
    //   }
    // });

  </script>
</html>
```

- Now we move to React.JS for the real Front-End
