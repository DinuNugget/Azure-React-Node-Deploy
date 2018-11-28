# Guide to Deploying Your Web App onto a Server

In this guide, we will start a minimalistic node server and try to deploy it on Azure.

## Content
* What is "Deployment"?
* What is Azure?
* Get a free student account on Azure
* Set Up The Server And Deploy


## What is "Deployment"?

Deployment is to "bring resources into effective action". 
In the context of web development, deployment simply means 
getting the our web app to run on an actual server 
and allowing other people to access your page publically.

An important difference is that you access your page with a URL like http://localhost:3000
before deployment. 
After deployment, people can access your page with an actual URL like http://www.example.com.

However, since your server need to run 24/7, so that people can access your page whenever,
you cannot use your laptop/computer as the server since it might disconnect from the Internet
or need to be turned off from time to time.

If so, we need to borrow other people's computer that will run 24/7.
Who will offer services like that? Maybe a tech giant like Microsoft?

In this tutorial, we will be focusing on how to deploy using the Microsoft Azure Cloud Computing Platform.

## What is Azure?

Azure is a cloud computing service created by Microsoft for building, testing, deploying, 
and managing applications and services through a global network of Microsoft-managed data centers.

For our purposes, Azure will allow us to rent one of their server and host our Node app there.

Did I just say rent? Yeah, it costs money. 
No body let other people use their computer for free. 
However, since we are broke college students, Microsoft offers us free trial accounts!

## Get a free student account on Azure
Let's get a free account on Azure so you can access the API.

Typically, you will have to pay to use their API. 
However, since we are students, Microsoft provides a free student account with $100 for you to try out the APIs.

Register here with this URL: Aka.ms/Azure4Students

* Register a Microsoft account with a __non-UCLA email__. Or just login if you have an existing one. 

* After that, we have to prove that we are students by entering our UCLA email.

* Login into your UCLA email and check the verification email.

## Set Up The Server And Deploy
### Initialize a Github Repo to put our server files.

We create a folder and initialize it with git.
Then, we initialzie it with npm.
Lastly, install the express package to start the server.

```
mkdir my-server
cd my-server
git init

npm init 
npm install --save express
```
Create a index.js file which will be our server

`index.js`
```js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.json({ message: 'Hello World' });
})

const PORT = process.env.PORT || 1337;
app.listen(PORT, () => {
    console.log('listening on', PORT);
});
```

This is a extremely minimalistic server. 
The only thing that might be strange is the PORT number.
```js
const PORT = process.env.PORT || 1337;
```
In this line, we are setting PORT as `process.env.PORT`, which is an environment variable.
The reason that we are doing this is that the Azure computer might allocate a specific PORT for our web app.
Therefore, we need to read the pre-configured PORT number which in the `process.env`, 
which is the 'environment'.

However, if the PORT number is not being set in the environment, we will default choose 1337.

Do a quick test to see if our server works.

```
node index.js
```

In our `package.json`, we need to define a start script.
```JSON
  "scripts": {
    "start": "node index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  }
```
The new `start` command is the command that the Azure computer runs to start our server.
Meaning, `npm run start` is the same as `node index.js` now.

Let's create a new github repository in your github account. 

Then, let's push our change to the new repository.
```
git remote add origin https://github.com/<your-github-name>/<your-repo-name>.git
git add index.js
git commit -m "first commit"
git push
```

### Actual Deployment
* Go to our Azure portal, click on __Create a resource__ on the left menu. 
* Select __Web App__.
* Enter App Name, choose carefully since your URL will be "\<your-app-name>.azurewebsites.net"
* Subscription should be __Azure for Students__.
* Resource Group should be __Create new__.
* OS should be __Linux__
* Publish should be __Code__
* Runtime Stack should be __Node.js 10.10__, or any other Node version that works.

After you create your resource group, it should take a while for it to show up on the Portal Home page.

* Click on the new resource group (App Service) you created.
* Click on __Deployment Center__.
* Select __Github__ and click Authorize.
* In build provider, select __App Service Kudu build server__.
* In configure, select yourself as the organization, select the repo that you just created. 
Branch is __master__.

Click ok and wait for deployment to finish. 
You should be able to it at http://\<your-app-name>.azurewebsites.net

Now, you know how to set up and simple server. Any more complex server with one entry file is just as easy!

## How do you deploy React app then?
If you were using `create-react-app` to create your React application, 
you would find that you run `npm start` and a server is some how set up for you 
and you can visit the page at `localhost:3000`

If you think about frontend for a second, 
we know that all webpages boils down to 3 static things, HTML, CSS and JavaScript.

React offers you great features like hot reloading by running the local server. 
However, to get a production-ready and optimized React we page. 
You can compile it to just HTML, CSS and JavaScript.

Let's do a quick demo.

```
create-react-app demo
cd demo
npm start 
```

This should start the react server at localhost:3000.
We know that React gives this default page to us. 
Now, Let's try to deploy this defualt page onto Azure.

First, we create the static build.

```
npm run build
```
After the program finishes, you should see a new folder called `build`
pop up in under your React app directory.
The `build` directory contains the static HTML, CSS, JavaScript needed for the frontend.

Now we copy this `build` folder to some other directory.
We will create a new Github repository, push this build folder up to our Github repository.

Create a new Azure App Service.


* Go to our Azure portal, click on __Create a resource__ on the left menu. 
* Search for __Web App__.
* Enter App Name, choose carefully since your URL will be "\<your-app-name>.azurewebsites.net"
* Subscription should be __Azure for Students__.
* Resource Group should be __Create new__.
* OS should be __Linux__
* Publish should be __Code__
* Runtime Stack should be __Node.js 10.10__, or any other Node version that works.




## Resources

* [Create Node.js app - offcial Azure Documentation](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-get-started-nodejs)
* [Deploy Continuously with Github](https://docs.microsoft.com/en-us/azure/app-service/app-service-continuous-deployment)
