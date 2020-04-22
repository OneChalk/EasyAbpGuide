# Getting Start to Develop Modules
This article will show you how to develop ABP modules that follow the [Module Development Specification](Module-Development-Specification.md) document.

## Step 1: Generate a new ABP module

Use ABP CLI to generate a new ABP module solution with the name prefix `EasyAbp.`, see: https://docs.abp.io/en/abp/latest/CLI#new.

## Step 2: Complete the project structure

1. In every project (except Web), create folders according to namespace and move all the original files and folders in, for example: `EasyAbp/(Abp)/MyModuleName/`.

2. Search `<RootNamespace>EasyAbp.MyModuleName</RootNamespace>` in src folder and replace with `<RootNamespace />`.

3. Move all the files in `EasyAbp.MyModuleName.Domain.Shard/EasyAbp/MyModuleName/Localization/MyModuleName/` to `EasyAbp.MyModuleName.Domain.Shard/EasyAbp/MyModuleName/Localization/`.

4. Open **MyModuleNameDomainSharedModule.cs**:
  * Change `options.FileSets.AddEmbedded<MyModuleNameDomainSharedModule>("EasyAbp.MyModuleName");` to `options.FileSets.AddEmbedded<MyModuleNameDomainSharedModule>();`.
  * Change `.AddVirtualJson("/Localization/MyModuleName");` to `.AddVirtualJson("/EasyAbp/MyModuleName/Localization/MyModuleName");`.
  * Change `options.MapCodeNamespace("MyModuleName", typeof(MyModuleNameResource));` to ``options.MapCodeNamespace("EasyAbp.MyModuleName", typeof(MyModuleNameResource));``.

5. Open **EasyAbp.MyModuleName.Domain.Shared.csproj**:
  * Change `<EmbeddedResource Include="Localization\MyModuleName\*.json" />` to `<EmbeddedResource Include="EasyAbp\MyModuleName\Localization\*.json" />`.
  * Delete other unused `<EmbeddedResource ... />` configurations.

## Step 3: Adjust Web project

> Tip: Skip this step if you do not need Web project.

1. **Pages** folder should have only **MyModuleName** folder, the folders of other items should be in the **MyModuleName** folder. If you adjust the structure as above, ensure the namespaces are correct.

2. Ensure the ModalManager object in **index.js** has the correct path, for example: `new abp.ModalManager(abp.appPath + 'MyModuleName/MyEntities/MyEntity/CreateModal');`

3. Ensure the `<abp-script ... />` and `<abp-style ... />` in **index.cshtml** have the correct path, for example: `<abp-script src="/Pages/MyModuleName/MyEntities/MyEntity/index.js" />`

## Step 4: Configure common.prop

Modify the **common.prop** file with reference to: https://github.com/EasyAbp/PrivateMessaging/blob/master/common.props.

## Step 5: Configure FodyWeavers.xml

After Building all the projects, **FodyWeavers.xml** will be generated to every project. Global replace `<ConfigureAwait />` to `<ConfigureAwait ContinueOnCapturedContext="false" />`.