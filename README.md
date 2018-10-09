# React-On-Rails-Setup-Guide

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
    "node": "8.7.0"
  },
  "scripts": {
    "build": "cd client && npm install && npm run build && cd ..",
    "deploy": "cp -a client/build/. public/",
    "postinstall": "npm run build && npm run deploy && echo 'Client built!'"
  }
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
    # this is the Ruby version of concurrently
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
9. foreman start -f Procfile.dev
```
