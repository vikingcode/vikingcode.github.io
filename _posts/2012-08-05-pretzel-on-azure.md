---
layout: page
title: Git deployment of Pretzel on Azure
---

> While this "works", Pretzel itself has a few bugs with the output paths. As such this is not *recommended*. I'd expect in the next week or two, the bugs should be ironed out.

Of the DVCS hosts, GitHub seems to be leading the pack and one of the reasons for that is [Jekyll](https://github.com/mojombo/jekyll/) - the system behind GitHub Pages. If you don't know, every repo on Github can have a "gh-pages" branch, which lets you upload a static site, that can be then hosted at owner.github.com/projectName/. A static site sounds pretty horrible to use, until you separate the content out into something like individual [Markdown](http://daringfireball.net/projects/markdown/) pages. On push to Github, Jekyll 'bakes' the pages and transforms them into HTML. Currently, even this site is powered by Jekyll.

Jekyll is nice but if you're not a Rubyist or if you don't like Liquid (the templating engine that Jekyll uses) it may not be *as* nice to you. One of the early projects of [Code52](http://code52.org/) was [Pretzel](http://code52.org/pretzel/) which started off as a .NET app with Jekyll compatibility. Since then, Razor has been added as well as other enhancements. The "problem" with Pretzel was that you'd have to prebake the site and commit the site to Git, rather than just the "source" files.

Azure "relaunched" a few months back, including a new product "Windows Azure Web Sites" (WAWS). WAWS is essentially Microsoft's version of AppHarbor or Heroku - Git (or TFS, or FTP) deployed sites "to the cloud". WAWS lets you use a whole stack of web tech - .net, node.js, java, php, python and potentially more.

The "build" process on .NET projects is:  

* Build the projects
* Copy build output to web server

Whereas on PHP/Node/etc projects

* Copy output to web server.

The challenge was finding the appropriate "type" of project for WAWS to compile (in this case, pipe out to Pretzel to bake the site). The tricky part is in a .NET project, that would mean you would have to reference *every* file, every new post, and so on. A tedious prospect. However, the less used "ASP.Net Web Site" - not to be confused with the compiled "Web Application" - just references a folder.

The solution ended up being a two project solution - a class library with nothing in it which simply calls Pretzel (Shim), and the "Web Site" (Sham) that points to the Pretzel output. The project structure looks like:

![](/images/postimages/shimsham.png)

###Shim.sln
	
	Microsoft Visual Studio Solution File, Format Version 11.00
	# Visual Studio 2010
	Project("{E24C65DC-7377-472B-9ABA-BC803B73C61A}") = "Sham", "_source\_site", "{F74001E8-C911-4A56-A200-7F6B85564035}"
		ProjectSection(WebsiteProperties) = preProject
			TargetFrameworkMoniker = ".NETFramework,Version%3Dv4.0"
			Release.AspNetCompiler.VirtualPath = "/"
			Release.AspNetCompiler.PhysicalPath = "_source\_site"
			Release.AspNetCompiler.TargetPath = "shim"
			Release.AspNetCompiler.Updateable = "true"
			Release.AspNetCompiler.ForceOverwrite = "true"
			Release.AspNetCompiler.FixedNames = "false"
			Release.AspNetCompiler.Debug = "False"
			VWDPort = "22490"
			DefaultWebSiteLanguage = "Visual C#"
		EndProjectSection
	EndProject
	Project("{FAE04EC0-301F-11D3-BF4B-00C04F79EFBC}") = "Shim", "Shim.csproj", "{08524E18-F666-4F41-93D5-6AB6D97A58E2}"
	EndProject
	Global
		GlobalSection(SolutionConfigurationPlatforms) = preSolution
			Release|Any CPU = Release|Any CPU
		EndGlobalSection
		GlobalSection(ProjectConfigurationPlatforms) = postSolution
			{08524E18-F666-4F41-93D5-6AB6D97A58E2}.Release|Any CPU.ActiveCfg = Release|Any CPU
			{08524E18-F666-4F41-93D5-6AB6D97A58E2}.Release|Any CPU.Build.0 = Release|Any CPU
		EndGlobalSection
		GlobalSection(SolutionProperties) = preSolution
			HideSolutionNode = FALSE
		EndGlobalSection
	EndGlobal

###Shim.csproj

{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="4.0" DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <Configuration Condition=" '$(Configuration)' == '' ">Release</Configuration>
    <Platform Condition=" '$(Platform)' == '' ">AnyCPU</Platform>
    <ProductVersion>8.0.30703</ProductVersion>
    <SchemaVersion>2.0</SchemaVersion>
    <ProjectGuid>{08524E18-F666-4F41-93D5-6AB6D97A58E2}</ProjectGuid>
    <OutputType>Library</OutputType>
    <AppDesignerFolder>Properties</AppDesignerFolder>
    <RootNamespace>Shim</RootNamespace>
    <AssemblyName>Shim</AssemblyName>
    <TargetFrameworkVersion>v4.0</TargetFrameworkVersion>
    <FileAlignment>512</FileAlignment>
  </PropertyGroup>
  <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|AnyCPU' ">
    <DebugType>pdbonly</DebugType>
    <Optimize>true</Optimize>
    <OutputPath>bin\Release\</OutputPath>
    <DefineConstants>TRACE</DefineConstants>
    <ErrorReport>prompt</ErrorReport>
    <WarningLevel>4</WarningLevel>
  </PropertyGroup>
  <ItemGroup>
    <Reference Include="System.Core" />
    <Compile Include="Shim.cs" />
    <Content Include="Pretzel.exe" />
  </ItemGroup>
  <Import Project="$(MSBuildToolsPath)\Microsoft.CSharp.targets" />
  <Target Name="AfterBuild">
    <Exec Command="pretzel.exe bake -t Razor -d _source" />
  </Target>
</Project>
{% endhighlight %}

###Shim.cs	

{% highlight csharp %}
namespace Shim
{
	//Without a CS file in a CSProj, MSBuild will fail throwing CS2008 
    class Shim
    {
    }
}
{% endhighlight %}