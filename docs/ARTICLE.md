## Introduction

This tutorial aims to show you how to deploy a [Blazor](https://dotnet.microsoft.com/en-us/apps/aspnet/web-apps/blazor) WebAssembly app on Netlify, a hosting platform with a ton of features. 

Here's a short video demo of the app.

%[https://www.youtube.com/watch?v=-0HHEt6j2f0]

Blazor is a feature of the ASP.NET framework that allows you to run client-side C# code directly in the browser, using [WebAssembly](https://webassembly.org/). Blazor can run without any server-side code. It's also open-source and free.

You will build an app using Blazor. Deploy to Netlify using Git and Github.

## Prerequisites

- [.NET SDK](https://dotnet.microsoft.com/download)
- Git
- Github Account
- Netlify Account

>**Note:**
>
>[The source code for this tutorial is available on Github.](https://github.com/Marktawa/silverfox)

## Set up Project Directory

Open up your terminal and create an empty project directory to store your project. This folder would serve as the root folder to store all your source code and config files. I will name my project directory `silverfox`.

```bash
$ mkdir silverfox
```

Change the directory to the project directory `silverfox`.

```bash
$ cd silverfox
```

Initiate your project directory as a git repository.

```bash
/silverfox $ git init
```

Generate a `.gitignore` file for dotnet applications within the root of your project directory.

```bash
/silverfox $ dotnet new gitignore
```

With that your project directory is set. Next, you will create and build your Blazor app.

## Create Blazor WebAssembly App

Execute the following command to create and build a Blazor WASM app.

```bash
/silverfox $ dotnet new blazorwasm
```

Run the application.
```bash
/silverfox $ dotnet run
```
Navigate to [localhost:5024](http://localhost:5024) in your browser to see it live.

![Live Blazor app running in your browser](https://www.dropbox.com/s/3e01x6fhhx8fs17/blazor-app.png?raw=1)

## Set up Netlify configuration file

We have seen that the app runs locally. However, for it to run on Netlify a configuration needs to be set before you deploy the app. This prevents any 404 issues.

Stop your running Blazor app by pressing `Ctrl + C` on your keyboard. 

Create a [Netlify configuration file](https://docs.netlify.com/configure-builds/file-based-configuration) named `netlify.toml` in the root of your project directory.

```bash
^C
/silverfox $ touch netlify.toml
```

Copy the following code to `netlify.toml`:

```toml
# ./netlify.toml

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

This a rewrite rule that redirects any request from `/*` to go to `/index.html` for your pages to render when hosted in Netlify. During the deployment process, Netlify will read `netlify.toml` and apply the configuration to your site.

## Push Blazor App to a GitHub remote repo

Stage and commit all the file changes that occurred in your project folder.

```bash
/silverfox $ git add .
/silverfox $ git commit -m "Initial commit"
```

Create a remote GitHub repository and add it as a remote repo to push to.

```bash
/silverfox $ git remote add origin <REMOTE_URL>
```

>Note:
>
>Replace <REMOTE_URL> with the URL to your GitHub repository.

Push the changes in your Git repository to your remote GitHub repository.

```bash
/silverfox $ git push origin main
```

## Set up Netlify site

- Log in to your [Netlify](https://app.netlify.com) account.
- Go to your **Team** page, select the **Add new site** drop-down and choose **Import an existing project**

![Import an existing project - Netlify](https://www.dropbox.com/s/ukfx9mnemk3i22c/import-an-existing-project-netlify-tinyp.png?raw=1)

- Click on the GitHub button. 

This will open a new window that will take you through giving Netlify permissions to your GitHub account. Give Netlify the necessary permissions and at least give Netlify access to the GitHub repository you just created.

![Select GitHub as Git provider](https://www.dropbox.com/s/rc3yalbz6bgi5e1/select-github-as-git-provider-tinyp.png?raw=1)
- Select the repository for your Blazor app.

![Select project repo](https://www.dropbox.com/s/gw0sdm6p7i5nfyg/pick-a-git-repo-tinyp.png?raw=1)

- Provide the build command and where the published files can be found. 

>**NOTE:**
>
>Netlify's build agent has all the required .NET tooling installed on their Ubuntu build machines.

- Leave the **Base directory** field blank unless your GitHub repo is a monorepo or your Blazor app was built in a sub-directory of your GitHub repo.

The **Base directory** is the directory where Netlify installs dependencies and runs your build command.

- In the **Build command** field enter `dotnet publish -c Release -o release`. 

`dotnet publish` creates a `publish` directory with a `wwwroot` folder. This `wwwroot` folder has everything needed for your deployment. The flag `-c Release` tells the CLI to build in Release configuration. The flag `-o release` tells the CLI to put the output in the release folder.

- In the **Publish directory** field enter `release/wwwroot`. Finally, click the **Deploy site** button.

![Build settings for your site](https://www.dropbox.com/s/3snmrksrikyh3k0/build-settings-for-your-site-tinyp.png?raw=1)

Netlify will create the site and you will be redirected to the site overview page. On this page, you can see the deploy status change in real-time. You can see the log output as Netlify builds your site. 

- Once your deployment finishes, a link to your site will be generated. Click on the link and you will see your app running in your browser hosted by Netlify.

![Live Blazor app hosted on Netlify](https://www.dropbox.com/s/bbymv8oqysytr4z/live-blazor-app-hosted-on-netify-tinyp.png?raw=1)

With that you should be done. Everything should be working fine.

## Conclusion

I hope this tutorial has helped you get started with Blazor and made the deployment process clear. Be on the look out for more tutorials related to Blazor, .NET, Netlify, and any other interesting web technologies on my blog.