# AspNetCore.SpaServices.Extensions.Vue

Start webpack development server with Hot Module Replacement with 
```csharp
spa.UseVueDevelopmentServer();
```

Example project: https://github.com/wikin/AspNetCore.SpaServices.Extensions.Vue-Example


## Limitations
- Node.js must be instaled on development machine
- Development must be at **HTTP** to make Hot Module Replacement works.

## Setting SPA source directory
Add a property to project file at begining.
```
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <SpaRoot>ClientApp\</SpaRoot>
  </PropertyGroup>
  ..........
```

## Endpoint Routing in AspNetCore
```csharp
public class Startup
  {
    public Startup(IConfiguration configuration)
    {
      Configuration = configuration;
    }

    public IConfiguration Configuration { get; }

    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
      services.AddControllers();
      services.AddSpaStaticFiles(configuration =>
      {
        configuration.RootPath = "ClientApp/dist";
      });
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
      
      // Production use webpack static files
      app.UseSpaStaticFiles();

      app.UseRouting();
      app.UseEndpoints(endpoints =>
      {
        endpoints.MapDefaultControllerRoute();
      });

      app.UseSpa(spa =>
      {
        spa.Options.SourcePath = "ClientApp";

        // Development use webpack development server with Hot Module Replacement
        if (env.IsDevelopment())
        {
          spa.UseVueDevelopmentServer();
        }
      });
    }
  }
  ```