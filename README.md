# Azure_DotNet_VSBuild
Use this task to build with MSBuild and set the Visual Studio version property. This task is only supported on agents running Windows.

# Azure_GenerateBuildSuffix_UpdateBuildNumber
This template enables to generate a build suffix for the package version and to update the build number

## Pipeline Requirements

The dotNet Module pipeline requires the following parameters to be defined:

Parameters:

| Name  | Displayname | type | Default | Values | Opional/Required | Comments |
| ------------- | ------------- | :-------------: | :-------------: | :-------------: | :-------------: | ------------- |
| solution | Solution | String | **\*.sln |  | Required | Specifies the solution for the task to use in the build process |
| vsVersion | Visual Studio Version | String | latest | latest, 17.0, 16.0, 15.0, 14.0, 12.0, 11.0 |  Optional | Allowed values: latest, 17.0 (Visual Studio 2022), 16.0 (Visual Studio 2019), 15.0 (Visual Studio 2017), 14.0 (Visual Studio 2015), 12.0 (Visual Studio 2013), 11.0 (Visual Studio 2012). The value of this input must match the version of Visual Studio used to create your solution. |
| msbuildArgs | MSBuild Arguments | String |  |  |  Optional | Passes additional arguments to MSBuild |
| platform | Platform | String |  |  |  Optional | Specifies the platform you want to build, such as Win32, x86, x64, or any cpu. If you are targeting an MSBuild project (.*proj) file instead of a solution, specify AnyCPU (no whitespace) |
| configuration | Configuration | String |  |  |  Optional | Specifies the configuration you want to build, such as debug or release. |
| clean | Clean | Boolean | false | true / false |  Optional | If set to false, the task makes an incremental build. This setting might reduce your build time, especially if your codebase is large. This option has no practical effect unless you also set the Clean repository to false. If set to true, the task rebuilds all of the code in the code projects. This is equivalent to the MSBuild /target:clean argument. |
| maximumCpuCount | Build in Parallel | Boolean | false | true / false |  Optional | If your MSBuild target configuration is compatible with building in parallel, you can check this input to pass the /m switch to MSBuild (Windows only). If your target configuration is not compatible with building in parallel, checking this option may cause your build to result in file-in-use errors, or intermittent or inconsistent build failures |
| restoreNugetPackages | Restore NuGet Packages | Boolean | false | true / false |  Optional | This input is deprecated. To restore NuGet packages, add a NuGet Tool Installer task before the build |
| msbuildArchitecture | MSBuild Architecture | String | x86 | x86, x64 |  Optional | Allowed values: x86 (MSBuild x86), x64 (MSBuild x64). Supplies the architecture (x86 or x64) of MSBuild to run. Because Visual Studio runs as a 32-bit application, you may experience problems when your build is processed by a build agent that is running the 64-bit version of Team Foundation Build Service. By selecting MSBuild x86, you may resolve these issues |
| logProjectEvents | Record Project Details | Boolean | true | true / false |  Optional | Records timeline details for each project |
| createLogFile | Create Log File | Boolean | false | true / false |  Optional | Creates a log file (Windows only) |
| logFileVerbosity | Log File Verbosity | String | normal | quiet, minimal, normal, detailed, diagnostic |  Optional | Use when createLogFile = true. Allowed values: quiet, minimal, normal, detailed, diagnostic. Specifies the verbosity level in log files |
| enableDefaultLogger | Enable Default Logger | Boolean | true | true / false |  Optional | If set to true, enables the default logger for MSBuild |
| customVersion | Custom Version | String |  |  |  Optional | Sets a custom version of Visual Studio. Examples: 15.0, 16.0, 17.0. The required version of Visual Studio must be installed in the system. Azure Pipelines: If your team wants to use Visual Studio 2022 with the Microsoft-hosted agents, select windows-2022 as your default build pool |

These parameters provide multiple use case options for the dotnet templates pipeline, enable/disable flags for the utilization of different templates as per the requirements.


## Use Cases

You can directly call a particular template as per the requirement. for example: 

  ```yaml
  # azure-pipeline.yml
  resources:
  repositories:
    - repository: Template
      type: github
      name: your_username/Azure_DotNet_VSBuild
      ref: <respective branch name>
      endpoint: 'githubServiceConnectioNname'

  steps:
  # passing the parameters
  - template: buildDotNetSolution.yml
    parameters:
      solution: '${{ parameters.solution }}' 
      vsVersion: '${{ parameters.vsVersion }}' 
      msbuildArgs: '${{ parameters.msbuildArgs }}'
      platform: '${{ parameters.platform }}' 
      buildConfiguration: '${{ parameters.buildConfiguration }}' 
      clean: '${{ parameters.clean }}' 
      maximumCpuCount: '${{ parameters.maximumCpuCount }}' 
      restoreNugetPackages: '${{ parameters.restoreNugetPackages }}' 
      msbuildArchitecture: '${{ parameters.msbuildArchitecture }}' 
      logProjectEvents: '${{ parameters.logProjectEvents }}' 
      createLogFile: '${{ parameters.createLogFile }}'
      logFileVerbosity: '${{ parameters.logFileVerbosity }}' 
      enableDefaultLogger: '${{ parameters.enableDefaultLogger }}' 
      customVersion: '${{ parameters.customVersion }}'                 

  ``` 
  
Make sure to adjust the repository name, branch name, and parameter values according to your project's requirements.
