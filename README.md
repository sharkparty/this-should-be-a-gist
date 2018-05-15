
# Running .NET Projects on OSX High Sierra (10.13.4)

**Disclaimer**: I have done the minimum amount of debugging to make sure you can pause execution and that breakpoints allow eval. That works pretty well. More to come on this.

### Prerequisites

 - [Rider (2018.1)](https://www.jetbrains.com/rider/) - Fast, powerful, cross-platform .NET IDE
 - [Mono (5.12.0)](http://www.mono-project.com/download/stable/) - Sponsored by [Microsoft](https://www.microsoft.com/), Mono is an open source implementation of Microsoft's .NET Framework based on the [ECMA](http://www.mono-project.com/docs/about-mono/languages/ecma/) standards for [C#](http://www.mono-project.com/docs/about-mono/languages/csharp/)and the [Common Language Runtime](http://www.mono-project.com/docs/advanced/runtime/).

**Note:** I had mono installed from installing Visual Studio 2017 Mac edition, so I included it here, but after installing Rider, it may already be on your system and configured.

### Installing/Configuring Rider (2018.1)
If you're just testing this, then there's not much of a need to activate the IDE, but if you need to activate the IDE for some reason (i.e. you already ran the trial) select server activation from the activation screen and add the following URL to the input:

`http://rmn-jetbrains.wsm.local:8080/licenseServer`

**Configuring TFS**

Once Rider is installed, make sure that the plugin for TFS is installed. You can add plugins without loading a project from the splash screen by clicking 'Configure' (bottom right-hand side) and then 'Plugins'. From there search for 'Visual Studio Team Services' (1.125.0) and make sure that's installed.

From here, you'll need to connect Rider to your MS Live account associated with the repository you're trying to clone. If you select 'Checkout from Version Control' from the dropdown and then select 'Team Services Git' it should prompt you to authenticate your account via oAuth. 

### Cloning a Project

Once your account is authenticated, you should see a list of all the repos associated with that account. Select the one you'd like to clone and make sure it's in an appropriate dev folder—I think by default Rider clones things to your root.

### Running a Project

Once the project finishes indexing, it should create a Run configuration automatically. While indexing the default run configuration may look like it's disabled or have a red checkmark over it. This is akin to a gas gauge on an old car that tells you bad news and then when you tap it a few times it tells you good news. Running it despite the "error" will likely run and serve your project.

*For the non-Jetbrains devs, all Jetbrains IDEs ship with a Run utility that you can configure to run build, serve, debug, etc. commands easily from the IDE. By default, you can find the Run config at the top right of your screen next to the big, green play button. The drop-down there is where you can add configurations for running various things. As a part of most C# projects I've tested, the IDE will infer the defaults of your local server config and add them. If not, HMU on Slack: @ks01741* 

**Note:** Watch out for port collisions locally. This is super obvious until it's not.

### Common Errors

`Invalid option 'portable' for /debug`: Being new to this project, I Googled around to fix this one. I think I used [this](https://developercommunity.visualstudio.com/content/problem/84142/csc-error-cs1902-invalid-option-portable-for-debug.html) article. My hunch is that on a PC this doesn't matter because your compiler is "forgivingly" set by VisualStudio, but on a Mac Rider isn't as forgiving so it uses whatever is defined by the `xxproj` file. You can fix this by upgrading your project's compiler to the latest version (2.8.0). [See "Adding/Updating a Project Package" below]

`Reference Wasn't Resolved By System, 'Device'` **This is any error about System.Device.Location or Geolocation** While I don't think this is the right solution, it is a solution. In our API, we use geolocation and that package—for unknown reasons ATM—isn't available in the mono port of C#. I need to dig into this further, but you can get around this up installing `System.Device.Location.Portable` (1.0.0) as a new package. 

`Reference Wasn't Resolved By Windows, 'Axure'` The solution to this is the same as above, but with a portable package for Axure. I think I used `Microsoft.WindowsAzure.Configuration` (1.0.0), but I don't remember clearly. The debugging process for this was to look at the `using` import and find a package that corresponded to that import; nothing fancy.

### Adding/Updating a Project Package

If you need to add a new dependency, select the root Solution (sln) file or Project (xxproj) file from the file navigator. From there, in the application menu bar, select Tools > NuGet > Manage Packages for x. You should see a window that lists all the packages visually. Using the search input, you can search for the package you'd like to add or update. 

If you're updating a package, select that package and look at the available versions. If adding a new dependency, search for it in the input and then add it to the project/solution.

**Disclaimer**: I'm still new to nuget & .Net.
