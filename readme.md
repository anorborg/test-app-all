Test Application-All
=====================

This repository allows you to make changes to referenced project sources instead of using packages. In order for this to work for a particular package/project reference, the `.csproj`
needs to be updated. We use the `Exists` condition to include the `ProjectReference` when the
`.csproj` is present and exclude the `PackageReference`. If the `.csproj` is NOT present the
`PackageReference` will be used. Below is an example:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  ...
  <ItemGroup>
    <PackageReference Condition="!Exists('[path-to-submoduled-project].csproj')" Include="[package-id]" Version="[package-version-selector]" />
  </ItemGroup>
  ...
  <ItemGroup>
    <ProjectReference Condition="Exists('[path-to-submoduled-project].csproj')" Include="[path-to-submoduled-project].csproj" />
  </ItemGroup>
  ...
</Project>
```

To include a new independent project/package combination:

- Create repository for the project
- Create a `.nuspec` for the project. At project root: `nuget spec`
- Once committed, add "Build Source" to "Build Servies" in MyGet to the project repository
- After the package builds, it will be available in the MyGet feed so you can add a `PackageReference`
- In combo repository (this one for instance), add project as submodule: `git submodule add http://github.com/realcrowd/[repo].git`
- Add project(s) to `.sln`
- Commit submodule references
- Edit any `.csproj` files that reference the new package using the `Exists` pattern described above
- To update, first commit to submodule:
```
$ cd path/to/submodule
$ git add <stuff>
$ git commit -m "comment"
$ git push
```
- Then update main project (like this one):
```
$ cd /main/project
$ git add path/to/submodule
$ git commit -m "updated my submodule"
$ git push
```




