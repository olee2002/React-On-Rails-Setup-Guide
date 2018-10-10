# React-On-Rails-Setup-Guide (Create-React-App)

React on Rails setup guide
```
1.rails new FOLDERNAME --api -d -T postgresql
    #--api stripped down version of Ruby on the rail (excludes views)
    #-T no test files 
```
```
2. cd FOLDERNAME
```
```
3. create-react-app client
```
```
4. create a package.json at the root folder and add below 
{
  "name": "YOUR PROJECT NAME",
  "engines": {
    "node": "8.7.0" # or latest and greatest version of node. 8.7.0 is already old at the time of this README. ;(
  },
  "scripts": {
    "build": "cd client && npm install && npm run build && cd ..",
    "deploy": "cp -a client/build/. public/",
    "postinstall": "npm run build && npm run deploy && echo 'Client built!'"
  }
  # copy and paste above into your file, I tried to type at times, never successful for some reasons.
}

    #this is for the static build file in production similar to the postinstall script in express.
```
```
5. add ("proxy": "http://localhost:3001",) the package json file in the client folder
{
  "name": "client",
  "version": "0.1.0",
  "private": true,
  "proxy": "http://localhost:3001",
  ...
}
    #This sets up the ability to call our Rails API without directly referencing localhost:3001
```
```
6. gem install foreman
    # this is the Ruby version of concurrently (being able to run two servers on one)
```
```
7. create Procfile.dev in the root folder and add below,
    web: sh -c 'cd client && PORT=3000 npm start'
    api: rails s -p 3001
```
```
8. bundle install (npm install express version)
```
```
9. foreman start -f Procfile.dev (Procfile is also important when you deploy it to Heroku)
```
```
gem "dotenv-rails", "~> 2.1", ">= 2.1.1" (If you use dotenv for your variables)
```
```
  host: <%=ENV['DATABASE_HOST']%>
  database: <%=ENV["DATABASE_NAME"]%>
  username: <%=ENV["DATABASE_USER"]%>
  password: <%=ENV["DATABASE_PASSWORD"]%>
```
```
rails g model Model name:string
rails g model SubModel model:references
rails g controller Name 
```
```
bundle exec rake db:create && bundle exec rake db:migrate (create database and run migration check migration before running this)
```
```
React on Rails Heroku Deployment
--------------------------------------------------------------------------------------------------------
1.heroku create YOUR-APP-NAME
    #WARNING!!! Be sure to replace "YOUR_APP_NAME" above with the project name from your package.json.
2.heroku buildpacks:add --index 1 heroku/ruby
heroku buildpacks:add --index 2 heroku/nodejs
3. create Procfile and add below
    web: rails s
4. git push heroku master
5. heroku addons:create heroku-postgresql:hobby-dev
6. heroku run rails db:migrate db:seed
7. heroku run rails c
--------------------------------------------------------------------------------------------------------
```

