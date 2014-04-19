---
layout: page
comments: true
title: How to actually reference Syncfusion WinRT PDFViewer
date: 2013-11-29
---
	
Syncfusion's WinRT "Essentials" are a set of pretty neat controls, including a PDFViewer. You might ask, why do you need a third party PDF Viewer when Windows 8.1's API includes one? Well, the built in one is a little bit arse. It is actually just a PDF to PNG renderer, which you can then display as an imagesource. You need to handle pagination, panning and zooming all by yourself. In contrast, SyncFusions control has all those controls built in, as well as vector zooming (I think).

However, it appears Syncfusion didn't generate a proper WinMD file nor create a "PDF Extension" for Visual Studio, so you have to manually add the references yourself (their regular WinRT controls are added via an extension though). The issue is when you want to target x86, x64 *and* ARM, you need to add the specific libraries for each platforms.

The way to do this (and to have it actually compile) is to use conditional references in the csproj

	  <ItemGroup Condition=" '$(Platform)' == 'x86' ">
	    <Reference Include="Syncfusion.DirectXWrapper.WinRT">
	      <HintPath>C:\Program Files (x86)\Syncfusion\Essential Studio\11.3.0.33\PrecompiledAssemblies\11.3.0.33\DirectX\8.1\x86\Syncfusion.DirectXWrapper.WinRT.winmd</HintPath>
	    </Reference>
	    <Reference Include="Syncfusion.SfPdfViewer.WinRT">
	      <SpecificVersion>False</SpecificVersion>
	      <HintPath>C:\Program Files (x86)\Syncfusion\Essential Studio\11.3.0.33\PrecompiledAssemblies\11.3.0.33\DirectX\8.1\x86\Syncfusion.SfPdfViewer.WinRT.dll</HintPath>
	    </Reference>
	  </ItemGroup>

	  <ItemGroup Condition=" '$(Platform)' == 'x64' ">
	    <Reference Include="Syncfusion.DirectXWrapper.WinRT">
	      <HintPath>C:\Program Files (x86)\Syncfusion\Essential Studio\11.3.0.33\PrecompiledAssemblies\11.3.0.33\DirectX\8.1\x64\Syncfusion.DirectXWrapper.WinRT.winmd</HintPath>
	    </Reference>
	    <Reference Include="Syncfusion.SfPdfViewer.WinRT">
	      <SpecificVersion>False</SpecificVersion>
	      <HintPath>C:\Program Files (x86)\Syncfusion\Essential Studio\11.3.0.33\PrecompiledAssemblies\11.3.0.33\DirectX\8.1\x64\Syncfusion.SfPdfViewer.WinRT.dll</HintPath>
	    </Reference>
	  </ItemGroup>

	  <ItemGroup Condition=" '$(Platform)' == 'ARM' ">
	    <Reference Include="Syncfusion.DirectXWrapper.WinRT">
	      <HintPath>C:\Program Files (x86)\Syncfusion\Essential Studio\11.3.0.33\PrecompiledAssemblies\11.3.0.33\DirectX\8.1\ARM\Syncfusion.DirectXWrapper.WinRT.winmd</HintPath>
	    </Reference>
	    <Reference Include="Syncfusion.SfPdfViewer.WinRT">
	      <SpecificVersion>False</SpecificVersion>
	      <HintPath>C:\Program Files (x86)\Syncfusion\Essential Studio\11.3.0.33\PrecompiledAssemblies\11.3.0.33\DirectX\8.1\ARM\Syncfusion.SfPdfViewer.WinRT.dll</HintPath>
	    </Reference>
	  </ItemGroup>

Then when submitting to the store, you have to build the platform specific (x86, x64 and ARM) packages rather than neutral. 

This is mostly one of those reference posts for myself so that next time I run into issues I can solve it.

###Update
[**Daniel Plaisted pointed out that you can just use $Platform instead**](https://twitter.com/dsplaisted/status/406276736070852608)

This means the XML becomes the following snippet rather than repeating three times.

	<ItemGroup>
		<Reference Include="Syncfusion.DirectXWrapper.WinRT">
			<HintPath>C:\Program Files (x86)\Syncfusion\Essential Studio\11.3.0.33\PrecompiledAssemblies\11.3.0.33\DirectX\8.1\$(Platform)\Syncfusion.DirectXWrapper.WinRT.winmd</HintPath>
		</Reference>
		<Reference Include="Syncfusion.SfPdfViewer.WinRT">
			<SpecificVersion>False</SpecificVersion>
			<HintPath>C:\Program Files (x86)\Syncfusion\Essential Studio\11.3.0.33\PrecompiledAssemblies\11.3.0.33\DirectX\8.1\$(Platform)\Syncfusion.SfPdfViewer.WinRT.dll</HintPath>
		</Reference>
	</ItemGroup>

