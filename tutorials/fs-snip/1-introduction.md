Static File Server From Custom Root Path
----------------------------------------

The following demonstrates the use of Freya and Owin.SelfHosting within a F# console 
project to serve static files relative to a given root path. The required nuget packages
are Freya and Microsoft.Owin.SelfHost in a F# Console project.

    module SelfHostWebSite
    
    open Freya.Core
    open Microsoft.Owin.Hosting
    open System.IO
    open System
    open Freya.Lenses.Http

    let fileServer root = freya {
      let! reqPath = Freya.Lens.get Request.Path_      
      let path =
        match reqPath.Substring(Path.GetPathRoot(reqPath).Length).TrimStart() with
        | ""  -> Path.Combine(root, "index.html")
        | q   -> Path.Combine(root, q)
      let (code, phrase, bytes) =
        try
          match File.Exists path with
          | true  -> (200, "OK"                     , File.ReadAllBytes(path))
          | _     -> (404, "File Not Found"         , [||])
        with _    -> (501, "Internal Server Error"  , [||] )
      do! Freya.Lens.setPartial Response.StatusCode_ code
      do! Freya.Lens.setPartial Response.ReasonPhrase_ phrase
      do! Freya.Lens.map Response.Body_ (fun s -> s.Write(bytes, 0, bytes.Length); s)
      }

    type Startup() =
      member __.Configuration() =
        OwinAppFunc.ofFreya (fileServer @"C:\SiteContent\")

    [<EntryPoint>]
    let main _ =
      try
        let url = "http://localhost:8080"
        let _ = WebApp.Start<Startup> (url)
        [ 
          "Serving site at " + url
          "Press ENTER to cancel"
        ] |> List.iter Console.WriteLine
        let _ = Console.ReadLine()
        0
      with x ->
        Console.WriteLine()
        Console.WriteLine x.Message
        Console.WriteLine()
        let _ = Console.ReadLine()
        1
