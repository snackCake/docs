# Getting Started

## <a name="wiki-overview"></a> Overview

Thanks for wanting to try out Broadleaf Commerce! By following this tutorial, you'll have your very own eCommerce site up and running in no time. We'll get your environment up and running, show you where things live, and walk you through a few examples of some of the cool things you can do with Broadleaf.

## <a name="wiki-prerequisites"></a> Prerequisites

- First, you'll need Java 7  [Java 7 JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)
    
- You will also need the latest version of Maven (at least version 3.1, version 3.2 onwards is recommended), which you can get [on the official Apache Maven site](http://maven.apache.org/download.html)  

- a Java development enviromnent (for this walkthrugh, we assume Eclipse Luna), but you can easily adapt these instructions to the IDE of your preference. [Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar)  
    
- Our sample Broadleaf package [Broadleaf Eclipse Workspace](http://www.broadleafcommerce.org/workspace-download?workspaceVersion=DemoSite-3.1.12-GA-eclipse-workspace.zip&docsVersion=current)

## <a name="wiki-gs-whats inside"></a>What is inside the package

  - Our sample Broadleaf contains a working shopping cart website. This website is adequately called **site** and it is uncluded in the package as an Eclipse project.  
  - In order to manage the contents of that project, you need an administration application. This application is also included as a website and project, and its name is **admin**.  
  - A third project, called **core** contains basic Broadleaf libraries, which are referenced by both *admin* and *site*. *Core* also contains other important stuff, like the initialization scripts for our content database.  
  - As these projects are built using Maven, it is useful to have a higher-level folder structure, where we keep things like the parent pom's, additional libraries, and, in general, any task or property that affects all projects globally. In our package, that super-folder is called ecommerce-website, and is, in itself, a "project".
  - In order to be truly standalobe, our demo package also runs its own, embedded Tomcat servers and its own hsql database. Aside from Maven, you don't really need any external tool or plugin to get your sites running!
  


 


## <a name="wiki-gs-configuration"></a> Configuration

We worked hard to make this demo as self-contained as possible.  
If you have Maven already installed in your system, the only thing you have to do, is to tell Broadleaf where your Maven is installed.  
  
The rest of the steps described here, are largely optional, platform-dependent housekeeping tips, aimed at making your development experience easier, but in no way mandatory.  

####1.Copy the Files
Copy the DemoSite folder inside from downloaded package, to anywhere on your disk. 

####2.Start Eclipse and create a brand new workspace
Create a brad new Eclipse workspace *(create it anywhere on your disk, **except** inside the DemoSite folder itself)*. There is a couple of ways to achieve this, but from inside Eclipse you can do **File->Switch Workspace->Other**, and then create or select an empty directory.

####3.Generate the projects and import them into Eclipse
Sitting on the DemoSite folder, run the following commands

```
mvn:eclipse clean  

[wait until it finishes]...

mvn eclipse:eclipse
```

Then, again inside Eclipse, do **File->Import->Maven->Existing Maven Projects**  


####4.Edit the Maven home property
Go to DemoSite/ecommerce-website/build.properties and edit the following property with your actual Maven location  

```
#required
maven.home=/Users/gdiaz/apache-maven-3.1.1
```
####5.Build the projects
Sitting on DemoSite, run the following command:

```
mvn clean install
```
####6.Group your Ant tasks
This is not mandatory, but it will make your life easier in Eclipse. Just open your Ant panel (**Window->ShowView->Ant**), and drag our relevant *build.xml* files into it. Those files are three:  

  - DemoSite/ecommerce-website/build.xml
  - Demosite/ecommerce-website/admin/build.xml
  - DemoSite/ecommerce-website/site/build.xml

That will make easier to invoke their respective Ant tasks from Eclipse. Notice that the build files are labelled *!BroadLeafDemoTools*, *admin*, and *site*, respectively.

  

####7.Changing the company name
This step is also entirely optional. All the code in our sample (package names, domain properties, etc) has an inverted-domain style name: "*com.mycompany* "  
 
You can change that name into the name of your own company. We also put together sample material for a fictional chili sauce retailer, **"Heat Clinic"**, so it would be nice for this example to have a *"com.heatclinic"* domain structure.

In order to globally change the company name, run the  **change-identifier** task inside !BroadLeafDemoTools. An input pop-up screen appears, just type in *"com.heatclinic"*.


## <a name="wiki-gs-running"></a> Running the projects

For your convenience, we packed our two main projects, *admin* and *site*, as web applications, each running into their own Tomcat embedded web server. You don't have to install anything nor perform any extra steps.  
Both applications read an embedded database (Hypersonic). We decided to allow this database to run independently, as a standalone service, so that it can be accessed by multiple connections (in case you want, for example, use a third-party tool to explore our databse schema.)

####1.Starting the database
The database starting and stopping scripts are Ant tasks inside the *site* Ant build. Locate and run **start-db** (double-clicling on it or right-click then **Run As->Ant Build**).
If everything goes well, you should see something like this on your console

```
Buildfile: /Users/gdiaz/blc-workspaces/community/DemoSite/site/build.xml
start-db:
     [echo] Starting Data Base...
BUILD SUCCESSFUL
Total time: 808 milliseconds
```
####2.Running the site project
On the same *site* Ant build, execute the **tomcat** task. If everything went well, you should see a lot of output going out to tour console, ending in something similar to this

```
[artifact:mvn] Feb 12, 2015 11:07:53 AM org.apache.coyote.AbstractProtocol start
[artifact:mvn] INFO: Starting ProtocolHandler ["http-bio-8080"]
[artifact:mvn] Feb 12, 2015 11:07:53 AM org.apache.coyote.AbstractProtocol start
[artifact:mvn] INFO: Starting ProtocolHandler ["http-bio-8443"]
```
This means that the embedded web server for your site is up and running!  

Go to your browser and enter the following address to check it out: 

**http://localhost:8080**

Isn't it nice?

####3.Running the admin project
In order to change the product, categories, etc your store sells, you must use the admin site. Without stopping your databse, go to the *admin* Ant build, and execute the **tomcat** task.  
If everything went well, you should see an output like this:

```
[artifact:mvn] INFO: Initializing Spring FrameworkServlet 'admin'
[artifact:mvn] Feb 12, 2015 11:18:54 AM org.apache.coyote.AbstractProtocol start
[artifact:mvn] INFO: Starting ProtocolHandler ["http-bio-8081"]
[artifact:mvn] Feb 12, 2015 11:18:54 AM org.apache.coyote.AbstractProtocol start
[artifact:mvn] INFO: Starting ProtocolHandler ["http-bio-8444"]
```

And, in order to check it out, open another browser window and go to

**http://localhost:8081/admin**

The username and password are **admin/admin**

####That's it! You have an up-and-running ecommerce website!


## <a name="wiki-gs-where-to-go"></a>Where to go from here

####Play around
Log into the **admin site**, and start playing around. Don't worry, you can't break anything, and you can easily recreate the original database contents from scratch.  

Some specific things you can start doing right away:  

 - **Load Some Data** (Try to add some additional products, all by yourself).   
 - [[Customize the UI | Customize UI For Heat Clinic Tutorial]]
 - **Configure checkout, taxes, payments**

####Browse and customize our code
We advise you to create 2 *"Java Working Sets"* in Eclipse, and put the ecommerce-website in one of them, and the 3 child projects *admin*, *site*, and *core*, in the other. Then switch to the latter Java Working Set, since we don't need the upper ecommerce-website folder anymore after setup.  
 
Broadleaf is higly configurable and extensible! When you are ready to get your feet wet on Broadleaf, here are some suggested activities:  

 - [[Store additional customer properties | Adding Customer Attribute Tutorial]]
 - Modify the registration form to prompt for user for referral code
Store the code in a CustomerAttribute
 - [[Extend the Customer entity | Extending Customer For Heat Clinic Tutorial]]
Add a few properties to the Customer to keep track of Heat Clinic metrics
 - [[Hook into the order submit workflow | Order Submit Workflow For Heat Clinic Tutorial]]
Keep track of how hot your customers like their sauces
 - [[Hook into the add to cart workflow | Add To Cart Workflow For Heat Clinic Tutorial]]
Learn about workflows and the add to cart workflow and set up a custom activity

####Install JRebel
If you want your development environment to look more like ours, install [[JRebel | JRebel Setup]]. JRebel is a very useful tool, that allows you to hot-deploy most of your Java code changes, without the need ot running Maven commands every time.

####Switch to MySQL
The embedded database that we include in our download, is just for sample purposes. If you want to get serious about using Broadleaf, we suggest to start using MySQL as your back-end. We created a [[useful tutorial | Switch To MySQL Tutorial]] to help you along the way.