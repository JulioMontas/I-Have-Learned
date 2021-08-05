- Make a new folder
```
mkdir Rails_api
```

- Make a Rails skeleton name `hellocode`
```
rails new hellocode --database=postgresql
```

- Going inside the rails folder
```
cd hellocode
```

- Create the table name
```
rails g model Language name author:string like:integer year:integer mainParadigm:string paradigm:string platform:string generation:string officialWebsite:string fileExtensions:string whoUse:string information:text moreInfo:text codeExample:text influencedBy:string influenced:string tutorials:string libraries:string references:string cheatsheet:text comment:text
```

- Create another table
```
rails g model Account first_name:string last_name:string user_name:string avatar_url:string gravatar_id:integer user_url:string followers_url:string following_url:string site_admin:boolean like:integer comment:text
```

- To create the table
```
rake db:create
```

- And then
```
rake db:migrate
```

- Copy and paste the text below to `db/seeds.rb`
```Ruby
Language.delete_all
Account.delete_all

Language.create!([
  {
    id: 1,
    name: "Javascript",
    author: "Brendan Eich",
    like: 0,
    year: 1995,
    mainParadigm: "Multi-paradigm",
    paradigm: ["Scripting", "Object-oriented", "Imperative", "Functional"],
    platform: ["Cross-platform"],
    generation:"blue",
    officialWebsite:[
      {name:"Developer.mozilla.org", url:"https://developer.mozilla.org/en-US/docs/JavaScript"}
    ],
    fileExtensions:[".js"],
    whoUse:[
      {name:"jQuery", url:"http://jquery.com/"},
      {name:"Gmail", url:"http://gmail.com/"},
      {name:"Google Analytics", url:"http://www.google.com/analytics/"},
      {name:"SocialHistory.js", url:"http://www.azarask.in/blog/post/socialhistoryjs/"},
      {name:"baroque.me", url:"http://baroque.me/"},
      {name:"Paper.js", url:"http://paperjs.org/"}
    ],
    information:"JavaScript, not to be confused with Java, was created in 10 days in May 1995 by Brendan Eich, then working at Netscape. JavaScript was not always known as JavaScript: the original name was Mocha, a name chosen by Marc Andreessen, founder of Netscape. In September of 1995 the name was changed to LiveScript, then in December of the same year, upon receiving a trademark license from Sun, the name JavaScript was adopted.",
    moreInfo:"This was somewhat of a marketing move at the time, with Java being very popular around then. In 1996 - 1997 JavaScript was taken to ECMA to carve out a standard specification, which other browser vendors could then implement based on the work done at Netscape. The work done over this period of time eventually led to the official release of ECMA-262 Ed.1: ECMAScript is the name of the official standard, with JavaScript being the most well known of the implementations. ActionScript 3 is another well-known implementation of ECMAScript, with extensions (see below). The standards process continued in cycles, with releases of ECMAScript 2 in 1998 and ECMAScript 3 in 1999, which is the baseline for modern day JavaScript. The 'JS2' or 'original ES4' work led by Waldemar Horwat (then of Netscape, now at Google) started in 2000 and at first, Microsoft seemed to participate and even implemented some of the proposals in their JScript.net language.",
    codeExample:"document.write('Hello World!');",
    influencedBy:[
      {name:"C", url:"langs/c"},
      {name:"Java", url:"langs/java"},
      {name:"Perl", url:"langs/perl"},
      {name:"Python", url:"langs/python"},
      {name:"Scheme", url:"langs/scheme"},
      {name:"Self", url:"langs/self"}
    ],
    influenced:[
      {name:"ActionScript", url:"langs/actionScript"},
      {name:"CoffeeScript", url:"langs/coffeeScript"},
      {name:"Dart", url:"langs/dart"},
      {name:"JScript .NET", url:"langs/jscriptnet"},
      {name:"Objective-J", url:"langs/objectivej"},
      {name:"QML", url:"langs/qml"},
      {name:"TIScript", url:"langs/tiscript"},
      {name:"TypeScript", url:"langs/typescript"}
    ],
    tutorials:[
      {name:"Mozilla JavaScript Guide", url:"https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide"}
    ],
    libraries:[
      {name:"Closure", url:"https://developers.google.com/closure/library/"},
      {name:"Three.js (3D library)", url:"https://github.com/mrdoob/three.js/"}
    ],
    references:[
      {name:"What is Javascript?", url:"https://www.w3.org/community/webed/wiki/A_Short_History_of_JavaScript"},
      {name:"Google Releases JavaScript Packages That Made Gmail, Docs, Maps", url:"https://www.rustybrick.com/google-releases-tools-that-made-gmail--docs--maps.html"}
    ],
    cheatsheet:[
      {syntax:"comments", example:"", description:""},
      {syntax:"hello world", example:"", description:""},
      {syntax:"booleans", example:"", description:""},
      {syntax:"variable", example:"", description:""},
      {syntax:"while loop", example:"", description:""}
    ],
    comment:[
      {userID: 1, commentsID:1, body:"This sucks"}
    ]
  }
])

Account.create!([
  {
    first_name: "Julio",
    last_name: "Montas",
    user_name: "JulioNYC",
    avatar_url: "https://helloco.de/users/img/default.jpg",
    gravatar_id:"",
    user_url:"https://helloco.de/users/JulioNYC",
    followers_url:"https://helloco.de/users/JulioNYC/followers",
    following_url:"https://helloco.de/users/JulioNYC/following/other_user",
    site_admin:"false",
    like:[
      {name:"Javascript", id:1 }
    ],
    comment:[
      {name:"Javascript", id:1, body:"This sucks"}
    ]
  }
])
```

- Build a relationship with `language.rb` and `account.rb` and also the array form for the JSON. Go inside the `app/models/language.rb` file to copy and paste the code below.
```Ruby
class Language < ActiveRecord::Base
  has_many :account
  serialize :paradigm, Array
  serialize :platform, Array
  serialize :officialWebsite, Array
  serialize :fileExtensions, Array
  serialize :whoUse, Array
  serialize :influencedBy, Array
  serialize :influenced, Array
  serialize :tutorials, Array
  serialize :libraries, Array
  serialize :references, Array
  serialize :cheatsheet, Array
  serialize :comment, Array
end
```

- Take the data of seeds.rb to place it in the database
```
rake db:seed
```

- Make the controller
```
rails g controller languages index
```

- Replace the text inside `app/controllers/application_controller.rb`
```Ruby
class ApplicationController < ActionController::Base
   protect_from_forgery with: :null_session
end
```

- Inside `config/routes.rb` remove `get 'language/index’` and add `resources :languages`
```Ruby
Rails.application.routes.draw do
  # create a route that responds to Get, Put, Delete, Created
  resources :languages
end
```

- Lets create a view. Inside `app/views/products` folder make a new file name
```
index.json.jbuilder
```

- Inside `app/views/languages/index.json.jbuilder` is going to create a Json object, is pretty much key and passing the value
```Ruby
json.languages @language do |language|
  json.name     language.name
  json.id      language.id
  json.author  language.author
  json.like     language.like
  json.year     language.year
  json.mainParadigm      language.mainParadigm
  json.paradigm      language.paradigm
  json.platform  language.platform
  json.generation     language.generation
  json.officialWebsite      language.officialWebsite
  json.fileExtensions  language.fileExtensions
  json.whoUse     language.whoUse
  json.information      language.information
  json.moreInfo     language.moreInfo
  json.codeExample      language.codeExample
  json.influencedBy  language.influencedBy
  json.influenced     language.influenced
  json.tutorials      language.tutorials
  json.libraries      language.libraries
  json.references  language.references
  json.cheatsheet  language.cheatsheet
  json.comment  language.comment
end
```

- Let’s go back to `app/controllers/languages_controller.rb` and add the code below
```Ruby
class LanguagesController < ApplicationController
  def index
    @languages = Language.all
  end
end
```

- In Terminal run `rails s` and in the Browser go to the URL of [http://localhost:3000/products.json]

- Inside `app/controllers/languages_controller.rb` update it to.
```Ruby
class LanguagesController < ApplicationController

  # Get All
  def index
    @language = Language.all
  end

  # GET one product
  def show
    @language = Language.find(params[:id])
  end


  # POST to create a product
  def create
    @language = Language.new(
    name: params[:name],
    author: params[:author],
    like: params[:like],
    year: params[:year],
    mainParadigm: params[:mainParadigm],
    paradigm: params[:paradigm],
    platform: params[:platform],
    generation: params[:generation],
    officialWebsite: params[:officialWebsite],
    fileExtensions: params[:fileExtensions],
    whoUse: params[:whoUse],
    information: params[:information],
    moreInfo: params[:moreInfo],
    codeExample: params[:codeExample],
    influencedBy: params[:influencedBy],
    influenced: params[:influenced],
    tutorials: params[:tutorials],
    libraries: params[:libraries],
    references: params[:references],
    cheatsheet: params[:cheatsheet],
    comment: params[:comment])

    if @language.save
      # Is rendering to a json files with 3 method, formats, handlers and status.
      render 'show', formats: [:json], handlers: [:jbuilder], status: 201
    else
      render json: {error: "Product could not be created"}, status: 422
    end
  end

  # UPDATE a product
  def update
    if @language.update_attributes(
      name: params[:name],
      author: params[:author],
      like: params[:like],
      year: params[:year],
      mainParadigm: params[:mainParadigm],
      paradigm: params[:paradigm],
      platform: params[:platform],
      generation: params[:generation],
      officialWebsite: params[:officialWebsite],
      fileExtensions: params[:fileExtensions],
      whoUse: params[:whoUse],
      information: params[:information],
      moreInfo: params[:moreInfo],
      codeExample: params[:codeExample],
      influencedBy: params[:influencedBy],
      influenced: params[:influenced],
      tutorials: params[:tutorials],
      libraries: params[:libraries],
      references: params[:references], 
      cheatsheet: params[:cheatsheet],
      comment: params[:comment])

      render 'show', formats: [:json], handlers: [:jbuilder], status: 200
    else
      render json: { error: "Story could not be updated." }, status: 422
    end
  end

  # Something Happen
  private
  def find_product
    @language = Language.find(params[:id])
  end

  # DELETE the id #
  def destroy
    @language = Language.find(params[:id])

    if @language.destroy
      render json: {}, status: 200
    else
      render json: { error: "Story could not be deleted." }, status: 422
    end
  end

end
```

- We need to add `Cors` to our code. Inside `reviewr/Gemfile`
```Ruby
#cors
gem 'rack-cors', :require => 'rack/cors'
```

- In terminal to install the `Gem`
```
bundle install
```

- Edit `config Application.rb`
```Ruby
require File.expand_path('../boot', __FILE__)

require 'rails/all'

# Require the gems listed in Gemfile, including any gems
# you've limited to :test, :development, or :production.
Bundler.require(*Rails.groups)

module Reviewr
  class Application < Rails::Application
    config.active_record.raise_in_transactional_callbacks = true
    config.middleware.insert_before 0, "Rack::Cors" do
      allow do
        origins '*'
        resource '/cors', :headers => :any, :methods => [:post, :put], :credentials => true, :max_age => 0
        resource '*', :headers => :any, :medthods => [:get, :post, :put, :options]
      end
    end
  end
end
```

- Make a new file `app/views/languages/show.json.jbuilder` to make a show view and product partial
```Ruby
json.product do
  json.partial! 'language', language: @language
end
```

- Make a new file `app/views/products/language.jbuilder`
```Ruby
json.name     language.name
json.id      language.id
json.author  language.author
json.like     language.like
json.year     language.year
json.mainParadigm      language.mainParadigm
json.paradigm      language.paradigm
json.platform  language.platform
json.generation     language.generation
json.officialWebsite      language.officialWebsite
json.fileExtensions  language.fileExtensions
json.whoUse     language.whoUse
json.information      language.information
json.moreInfo     language.moreInfo
json.codeExample      language.codeExample
json.influencedBy  language.influencedBy
json.influenced     language.influenced
json.tutorials      language.tutorials
json.libraries      language.libraries
json.references  language.references
json.cheatsheet  language.cheatsheet
json.comment  language.comment
```
- In Terminal type `rake routes` To see your routes

- Inside the root folder `mkdir front-end`

- Testing the front-end with a Ajax call `front-end/index.html`
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
