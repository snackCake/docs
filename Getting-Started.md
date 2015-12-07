# Getting Started

## <a name="wiki-overview"></a> Overview

Thanks for exploring Broadleaf Commerce!  With this tutorial, you'll have your eCommerce site launched in no time.  We'll get your environment up and running, show you where things live, and walk you through a few examples of some of the cool things you can do with Broadleaf.

## <a name="wiki-prerequisites"></a> Prerequisites

- First, download [the Java 8 Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

> [[Debugging Startup Issues]]

- You will also need the latest version of Maven (at least version 3.1, version 3.2 is recommended), which you can get [on the official Apache Maven site](http://maven.apache.org/download.html)

## <a name="wiki-eclipse"></a> IDE Setup

> Note: The first part of this Getting Started guide will cover using our prepackaged Eclipse workspace. If you are an advanced user and would prefer to set up the project yourself, please feel free to follow the [[Eclipse Setup]] or [[Intellij Setup]] guide.

Once you have the Java SDK installed, we're ready to get your IDE up and running. Let's start by downloading Eclipse.

Download Link: [Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunasr2)

Once Eclipse is done downloading, extract the archive and start it up. You'll be prompted for a workspace:

![Initial Prompt](gs-workspace-initial-prompt.png)

Go ahead and accept the initial value.

> Note: Don't save this as the default workspace -- we're going to be setting up a new workspace shortly.

Click on **Help --> Eclipse Marketplace** and search for **Maven Integration**. Make sure you pick the plugin provided by Eclipse.org as highlighted below:

![Maven Integration Plugin](gs-eclipse-install-maven.png)

Click **Install** followed by **Next**. After that, **Accept the License Terms** and **Finish**.

Once the plugin is done installing, you will be prompted to restart Eclipse. Go ahead and do so.

You'll once again be asked for a workspace. This time, we're going to pick a different one. To make getting started with Broadleaf as easy as possible, we've already set up a workspace with some reasonable defaults and tweaks to help you out. Let's download it!

Broadleaf 4.0 workspace Download Link: [Broadleaf Eclipse Workspace](http://www.broadleafcommerce.org/workspace-download?workspaceVersion=DemoSite-4.0.5-GA-eclipse-workspace.zip&docsVersion=current)

> Note: If you're not prompted for a workspace, simply go to **File --> Switch Workspace** and select the path

Extract this archive to the location you want your workspace to live in, and point Eclipse to the appropriate path. This time, your workspace should look like this:

![Eclipse Initial Workspace](gs-eclipse-hc-workspace.png)

Now we need to import the root ant targets. In the ant view pane, click the 'Add Build Files' button (the button farthest to the left). Select the build.xml from the root DemoSite project and then hit 'Ok':

![Root ant targets](gs-add-root-ant.png)

We now need to import the subprojects, core, site, and admin. We do this by going to **File --> Import**, and picking **Existing Maven Projects**, like this:

![Import Maven Projects](gs-import-maven-projects.png)

Click **Browse** and pick the `DemoSite` folder as the project root, and click **Open**

![Import Browse Folder](gs-import-browse-folder.png)

On this screen, make sure you check the **Add project(s) to working set** box and pick the `My Broadleaf Site` working set. Also make sure that all four project boxes are checked

![Import Finish](gs-import-finish.png)

> This process may take a while. You can see the progress in the bottom right of Eclipse or open the Maven Console for more detail progress reports.

Once the import is complete, you'll have to add two buildfiles, one for site and one for admin. Click on the **Add Buildfiles** button on the Ant pane

![Add Buildfile](gs-add-buildfile.png)

and select the two buildfiles to add:

![Add Buildfile Dialog](gs-add-buildfile-dialog.png)

The last thing to do is configure the path of your local maven installation. This is done in `DemoSite/build.properties` on the `maven.home` line.

> **NOTE Windows users must use forward slashes (/) for paths, not backslashes (\)**

That's it! Now you have your IDE completely setup to work with Broadleaf Commerce, and your workspace should look like this

![Workspace Complete](gs-workspace-complete.png)

## <a name="wiki-configuring-names"></a> Configuring Project Name

> **Note: Some have reported issues at this step. You can safely skip the "Configuring Project Name" step in its entirety. We are investigating.**

The workspace project and maven artifact all refers to "com.mycompany". For your convenience, we've provided an Ant task that will perform all necessary rename and move operations to customize the project to your organization. Simply run the `change-identifier` Ant task and type in a suitable name.

> **Note: The name should be two alphabetic strings separated by a dot. For example, "com.heatclinic"**

![Project Change Identifier](gs-project-change-identifier.png)

You will be prompted to confirm your selection. It should identify the Maven Group as the string you typed in and the Company Name as the part after the dot.

![Project Change Identifier Confirm](gs-project-change-identifier-confirm.png)

Once that task is run, **Right click inside Package Explorer --> Refresh**.

> Note: You will only be allowed to configure the project name via this script once.

![Project Refresh](gs-workspace-refresh.png)

## <a name="wiki-starting-site"></a> Running Site

Now that we have our workspace properly configured, we're able to get our demo site up and running. First, we will have to let Maven fetch the necessary dependencies and install our own project locally. To do that, **Right click on DemoSite --> Run As --> Maven Install**.

![Eclipse Maven Install](gs-eclipse-maven-install.png)

> Note: Running a Maven install is only necessary initially. To shorten your development iteration times, you utilize JRebel, which will allow you to modify files without having to restart the server. We've detailed out [[how to setup JRebel | JRebel Setup]] for you.

This process will take a few minutes to execute, and will end on the following success message:

```text
[INFO] Reactor Summary:
[INFO]
[INFO] ecommerce ......................................... SUCCESS [0.440s]
[INFO] core .............................................. SUCCESS [2.505s]
[INFO] admin ............................................. SUCCESS [3:37.628s]
[INFO] site .............................................. SUCCESS [24.109s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 4:05.052s
[INFO] Finished at: Mon Jul 30 10:01:19 CDT 2012
[INFO] Final Memory: 13M/81M
```

At this point, we're ready to start up! Let's run the `tomcat` Ant task for the site.

![Tomcat](gs-tomcat-site.png)

You'll see some logging messages in the Console scroll by, and eventually stop on

```text
[artifact:mvn] Apr 19, 2015 11:16:24 PM org.apache.coyote.AbstractProtocol start
[artifact:mvn] INFO: Starting ProtocolHandler ["http-bio-8080"]
[artifact:mvn] Apr 19, 2015 11:16:24 PM org.apache.coyote.AbstractProtocol start
[artifact:mvn] INFO: Starting ProtocolHandler ["http-bio-8443"]
```

That's it! The server's up! Let's check it out: <a href="http://localhost:8080/" target="_blank">http://localhost:8080/</a>

![Tomcat Site](gs-tomcat-admin.png)

## <a name="wiki-starting-admin"></a> Running Admin

Once the site has been started up, we can start up the admin as well

> Note: The site startup will conveniently populate some database tables, including the admin users tables. Therefore, the site must be running before starting up the admin.

This time, we'll hit the `tomcat` Ant task for the admin.

![Tomcat Admin](gs-tomcat-admin.png)

This console will end up at

```text
[artifact:mvn] Apr 19, 2015 11:16:24 PM org.apache.coyote.AbstractProtocol start
[artifact:mvn] INFO: Starting ProtocolHandler ["http-bio-8081"]
[artifact:mvn] Apr 19, 2015 11:16:24 PM org.apache.coyote.AbstractProtocol start
[artifact:mvn] INFO: Starting ProtocolHandler ["http-bio-8444"]
```

And now we can hit the admin! <a href="https://localhost:8081/admin" target="_blank">https://localhost:8081/admin</a>.  At the login prompt, enter the default username/password of **admin/admin** and you should see the below screen:

> Note: The admin to serves all of its pages over https by default. This means that you will encounter a security exception in your browser because the certificates do not match up.  You should be able to safely ignore this warning (when running locally).

![Tomcat Admin](gs-tomcat-admin.png)

> Any issues? Please come post in the [Broadleaf forums](http://forum.broadleafcommerce.org) - We would love to help you out!

## <a name="wiki-next-steps"></a> Next Steps

So now that you have your own Broadleaf site set up, what's next? We recommend getting familiar with the framework and starting to make your own personalizations. Here are some cool things to try out:

<div class="half-column">
    <h3> Configuration
        <span class="small">(What you need to do to get a real site running)</span>
    </h3>

    <ul>
        <li>
            [[Switching to MySQL | Switch To MySQL Tutorial]]
            <ul>
                <li>Learn how to use your own MySQL database server.</li>
            </ul>
        </li>
        <li>
            Load Data
            <ul>
                <li>Learn how to set up a clean slate for your products.</li>
            </ul>
        </li>
        <li>
            [[Customize the UI | Customize UI For Heat Clinic Tutorial]]
            <ul>
                <li> Learn about the Broadleaf UI strategy and patterns</li>
                <li> Customize key pieces of the Heat Clinic template and discover the Broadleaf Content Management System</li>
            </ul>
        </li>
        <li>
            Configure Checkout
            <ul>
                <li> Add a shipping partner</li>
                <li> Configure taxes</li>
                <li> Handle payments</li>
            </ul>
        </li>
        <li>
            [[Production Considerations]]
            <ul>
                <li> Ensure your security is set up properly</li>
                <li> Configure emails</li>
            </ul>
        </li>
</div>

<div class="half-column">
    <h3>Customization
        <span class="small">(Some fun things to try out to get your feet wet)</span>
    </h3>

    <ul>
        <li>
            [[Storing additional customer properties | Adding Customer Attribute Tutorial]]
            <ul>
                <li> Modify the registration form to prompt for user for referral code</li>
                <li> Store the code in a CustomerAttribute</li>
            </ul>
        </li>
        <li>
            [[Extend the Customer entity | Extending Customer For Heat Clinic Tutorial]]
            <ul>
                <li> Add a few properties to the Customer to keep track of Heat Clinic metrics</li>
            </ul>
        </li>
        <li>
            [[Hook into the order submit workflow | Order Submit Workflow For Heat Clinic Tutorial]]
            <ul>
                <li> Keep track of how hot your customers like their sauces</li>
            </ul>
        </li>
        <li>
            [[Hook into the add to cart workflow | Add To Cart Workflow For Heat Clinic Tutorial]]
            <ul>
                <li> Learn about workflows and the add to cart workflow and set up a custom activity</li>
            </ul>
        </li>
    </ul>
</div>

<div style="clear: both;">&nbsp;</div>
