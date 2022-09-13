# Demonstrate `MissingMethodException` for Json Extension Method
# AWS Lambda Simple SQS Function Project

## Reproduction steps
- Create new lambda SQS c# project
  - `dotnet new lambda.SQS`
- Add using for `System.Net.Http.Json`
- Utilize json extension method to parse json 
  ```
   var apiRequest = new HttpRequestMessage(HttpMethod.Get, "https://api.github.com/");
   var response = await _httpClient.SendAsync(apiRequest);
   var jsonContent = await response.Content.ReadFromJsonAsync<Dictionary<string, string>>();
  ```
- Run the lambda test tool and use the example SQS payload
- When executing code the caller function will not be able to enter a function body that references the `ReadFromJsonAsync` method call
- Stacktrace
```
System.MissingMethodException: Method not found: 'System.Threading.Tasks.Task`1<!!0> System.Net.Http.Json.HttpContentJsonExtensions.ReadFromJsonAsync(System.Net.Http.HttpContent, System.Text.Json.JsonSerializerOptions, System.Threading.CancellationToken)'.
   at lambda_json_missing_method.Function.ProcessMessageAsync(SQSMessage message, ILambdaContext context)
   at System.Runtime.CompilerServices.AsyncMethodBuilderCore.Start[TStateMachine](TStateMachine& stateMachine)
   at lambda_json_missing_method.Function.ProcessMessageAsync(SQSMessage message, ILambdaContext context)
   at lambda_json_missing_method.Function.FunctionHandler(SQSEvent evnt, ILambdaContext context) in /path/to/example/project/src/lambda-json-missing-method/Function.cs:line 37
   at Amazon.Lambda.TestTool.Runtime.LambdaExecutor.ProcessReturnAsync(ExecutionRequest request, Object lambdaReturnObject) in C:\codebuild\tmp\output\src220042227\src\Tools\LambdaTestTool\src\Amazon.Lambda.TestTool\Runtime\LambdaExecutor.cs:line 138
   at Amazon.Lambda.TestTool.Runtime.LambdaExecutor.ExecuteAsync(ExecutionRequest request) in C:\codebuild\tmp\output\src220042227\src\Tools\LambdaTestTool\src\Amazon.Lambda.TestTool\Runtime\LambdaExecutor.cs:line 62

```

## dotnet info
```
$ dotnet --info
NET SDK (reflecting any global.json):
 Version:   6.0.108
 Commit:    4e3a463d2b

Runtime Environment:
 OS Name:     fedora
 OS Version:  36
 OS Platform: Linux
 RID:         fedora.36-x64
 Base Path:   /usr/lib64/dotnet/sdk/6.0.108/

global.json file:
  Not found

Host:
  Version:      6.0.8
  Architecture: x64
  Commit:       55fb7ef977

.NET SDKs installed:
  6.0.108 [/usr/lib64/dotnet/sdk]

.NET runtimes installed:
  Microsoft.AspNetCore.App 6.0.8 [/usr/lib64/dotnet/shared/Microsoft.AspNetCore.App]
  Microsoft.NETCore.App 6.0.8 [/usr/lib64/dotnet/shared/Microsoft.NETCore.App]

Download .NET:
  https://aka.ms/dotnet-download

Learn about .NET Runtimes and SDKs:
  https://aka.ms/dotnet/runtimes-sdk-info
```