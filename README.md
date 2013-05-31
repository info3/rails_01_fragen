Create this Application
========================

    rails new fragen
    cd fragen
    rails g scaffold question question:string comments:text votes:integer
    rake db:migrate
    rails s

(edit root in config/routes.rb)

    rm public/index.html
    git init
    git add .
    git commit -m "initial commit"

Heroku (optional)
-------
(edit Gemfile to use pg in production)
(heroku toolbelt installieren: https://toolbelt.heroku.com)

    heroku create
    git push heroku master
    heroku run rake db:migrate
    heroku rename info3-questions
    heroku open


Heroku
======
This app is available on Heroku [http://info3-questions.herokuapp.com/](http://info3-questions.herokuapp.com/)

Add +1 Button
==============

add named route in config/routes.rb:

    match 'questions/:id/plusone' => 'questions#plusone', :as => :plusone

show routes with rake routes

add link in app/views/questions/index.html.erb:

    <td><%= link_to ' +1 ', plusone_path(question) %></td>


click on it. you get complaint that controller action does not exist.

create it in app/controllers/question_controller.rb

    def plusone
        @question = Question.find(request[:id])
        @question.votes = (@question.votes || 1) +1
        @question.save
      respond_to do |format|
        format.html { redirect_to questions_url, notice: "Plus one for '#{@question.   question}'."}
        format.json { render json: @questions }
      end
    end

To see the notice in the index, you need to add this line:

<p id="notice"><%= notice %></p>


Now, refactor: plusone should be a method of question: put it in app/models/question.rb and call it from the controller:

    def plusone
      self.votes= (votes || 0) +1
    end

