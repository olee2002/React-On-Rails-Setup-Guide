# React-On-Rails-Setup-Guide (Create-React-App)

React on Rails setup guide
```
1.rails new FOLDERNAME --api  -T -d postgresql ( if mysql do mysql)
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
4. create a package.json (npm init) in root(rails) and add below 
{
  "name": "YOUR PROJECT NAME",
  "engines": {
    "node": "10.11.0" 
  },
  "scripts": {
    "build": "cd client && npm install && npm run build && cd ..",
    "deploy": "cp -a client/build/. public/",
    "postinstall": "npm run build && npm run deploy && echo 'Client built!'"
  }
  # copy and paste the above, I tried to type a few times, never successful for some reasons.
}

  #this is for the static build file in production similar to the postinstall script in express.
```
```
5. cd client/ 
1) add ("proxy": "http://localhost:3001",) to package.json in client
{
  "name": "client",
  "version": "0.1.0",
  "private": true,
  "proxy": "http://localhost:3001",
  ...
}
   #This sets up the ability to call our Rails API without directly referencing localhost:3001

2)npm i axios react-router-dom (optional)

** Add below to index.html if you use bootstrap (optional: makes life easier)
3)<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css">
```
```
6. gem install foreman
    # this is the Ruby version of concurrently (being able to run two servers on one)
```
```
   gem "dotenv-rails", "~> 2.1", ">= 2.1.1" (If you use dotenv for your env variables)
```
```
7. create Procfile.dev in the root folder and add below,
    web: sh -c 'cd client && PORT=3000 npm start'
    api: rails s -p 3001
```
```
8. bundle install  & npm install 
```
```
9. foreman start -f Procfile.dev (Procfile is also important when you deploy it to Heroku)
```
```
 ** for remote database with ENV setting
  .env
  DATABASE_HOST=
  DATABASE_NAME=
  DATABASE_USER=
  DATABASE_PORT=
  DATABASE_PASSWORD=
  In database.yml
  host: <%=ENV['DATABASE_HOST']%>
  database: <%=ENV["DATABASE_NAME"]%>
  username: <%=ENV["DATABASE_USER"]%>
  password: <%=ENV["DATABASE_PASSWORD"]%>
```
```
10, basic model & controller creation with rails CLI
rails g model Model name:string
rails g model SubModel model:references
rails g controller api/Name 

Add has_many, dependent: :destroy(in the parent model, this might conflict with seeding)
```
```
11,bundle exec rake db:create && bundle exec rake db:migrate 
  #create database and run migration check migration before running this
  for local database rake db:create is required 
  (creating database but for remote, typically the database is already created so run db:migrate)
```
```
12.create routes 

namespace :api do
resources models do
resources sub-models
 end
end
```
```
13.Seed data / rake db:migrate (test each route one by one as you go)
```
```
netstat -ant | grep 3000
rails server -p 3001 (just testing api only)
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
