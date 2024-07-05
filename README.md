# Unity Package Template

This repository is a template for creating Unity packages. Unity packages are useful for creating separate features that can be easily imported into any other Unity project to reuse them. These packages can be automatically managed with the inbuilt Unity Package Manager, allowing developers to switch between different versions of the same package at any time. Packages can also define optional samples and their own dependencies that should be automatically downloaded alongside them. This template contains a complete Unity project, which can be used to develop and test the package, and then publish the inner package with three simple git commands.

## File structure

> `Assets` : all assets used for the development of the package are placed here </br>
>> Assets should adhere to the normal unity folder structure (Scene, Resources, Materials etc.), assets placed here can be used to develop and test the package without having the test assets be included in the published package </br>
>> `/UnityPackage` : all assets that should be part of the published package are placed underneath this directory </br>
>>> `/Editor` : all editor scripts such as custom editors or settings are placed here </br>
>>> `/Runtime` : all runtime scripts are placed here </br>
>>> `/Samples~` : optional samples of the package are placed here </br>

Only assets underneath the Assets/<strong>UnityPackage</strong> will be part of the published package. Assets that are only used for developing and testing the package should therefore be outside the <strong>UnityPackage</strong> folder. The Editor and Runtime directories inside the <strong>UnityPackage</strong> directory should contain assembly definitions to make sure the contained assets are only included in the corresponding platform. The Editor assembly definition, if the package should even use a Editor directory, should also have a one-directional GUID reference to the runtime assembly definition.

The root directory of the repository should contain a <strong>LICENSE</strong> and a <strong>README.md</strong> file to allow Github to index them easily. Alongside the <strong>package.json</strong> the <strong>UnityPackage</strong> directory should contain a <strong>LICENSE.md</strong>, <strong>README.md</strong> and <strong>CHANGELOG.md</strong> file, which are linked to in the <strong>package.json</strong> and can be from viewed inside the Unity Package Manager UI. 

For more information see [Unity Package Layout](https://docs.unity3d.com/Manual/cus-layout.html).

## Samples

Samples are optional features and assets that can be downloaded alongside the package from the Unity Package Manager. Samples can be used to provide example scenes, explanations, tests etc. for the users. Samples must be placed underneath the `Samples~` folder inside the package and be defined in the package.json. To prevent Samples from being imported automatically alongside the rest of the package, the Samples folder must contain a <strong>~</strong> character at the end of the folder name. This hides the folder from Unity, and allows users to selectively import samples from the Package Manager UI. Because the <strong>~</strong> must be added back to the samples folder, hiding the folder from Unity, it can be annoying to develop the samples in the provided template project. It is therefore recommended to have two separate branches (ex.: develop and main). All changes should first be made on develop and then be merged to the main branch, where the package can be published. On the develop branch the Samples directory should not contain the <strong>~</strong> character. Once a new sample is added to the project, a new commit should be made, where the <strong>~</strong> character is added to the folder name ONLY on the main branch. This enables you to develop the sample on the develop branch and have the <strong>~</strong> automatically be added everytime you merge your changes to the main branch.

`Samples` should have the following folder structure:

> `Samples~`
>> `/SAMPLE_NAME`
>>> normal Unity folder structure

For more information see [Unity Samples](https://docs.unity3d.com/Manual/cus-samples.html).

## Using the template

When using the template to create a new Unity package project, make sure to first adjust the Unity project version. Then update the <strong>Edit > Project Settings > Player</strong> CompanyName, Product Name and Version. Also update the <strong>UnityPackage/package.json</strong> values, like name (ex.: de.htw-berlin.centis.project-name), display name, version, URLs, author and samples. The version number in the Player settings and package.json should adhere to the SemVer format.

## Publishing

To publish the package it is recommended to make all changes on the develop branch, then merge the changes to the main branch and publish the package from there. Before publishing make sure to update the version number in the <strong>Edit > Project Settings > Player</strong> settings and the <strong>UnityPackage/package.json</strong>. Also make sure that if any samples are present, they have a <strong>~</strong> character at the end of the Samples directory name. 
Once the main branch and version is updated, the package can be published from the Git Bash tool. Using the commands:

```
git subtree split --prefix=Assets/UnityPackage --branch upm
git tag VERSION upm
git push origin upm --tags
```

the local package version can be published under a new tag on the Unity Package Manager (upm) branch. When using the commands make sure to replace the <strong>VERSION</strong> with the SemVer version number for the package. All files underneath the <strong>UnityPackage</strong> directory are then published with the Git tag on the upm branch. After publishing the branch, no commits must be made to the upm branch. To update the package, simply change the version number and run the commands with the new version again. The published tags and package version can be viewed on Github in the Release > Tag section.

To import the package into a new Unity project paste the package URL in the <strong>Window > Package Manager > + > Add package from git URL...</strong> field. The package URL is the HTTPS URL (ex.: https://github.com/FKI-HTW/UnityPackageTemplate.git) with a <strong>#upm</strong> for the most recently published version or <strong>#VERSION_TAG</strong> at the end. The complete URL for a specific version of a package could look something like this: https://github.com/FKI-HTW/UnityPackageTemplate.git#1.2.3.