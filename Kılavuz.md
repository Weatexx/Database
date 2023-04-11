dotnet add package Microsoft.EntityFrameworkCore 
dotnet add package Microsoft.EntityFrameworkCore.Design 
dotnet add package Pomelo.EntityFrameworkCore.MySql

dotnet tool install --global dotnet-ef

dotnet ef dbcontext scaffold "Server=localhost;User=root;Database=kitap_db; default command timeout=120; SslMode=none" "Pomelo.EntityFrameworkCore.MySql" -o Models/Entities

///////////// Models/dbcontext içerisinde
OnConfiguring içerisindeki aşağıdaki ifadeyi sil


///////////// appsettings.json içerisine "*" sonrasına virgül koy ve alttakini yaz
"ConnectionStrings": {
    "DefaultConnection": "Server=localhost;User=root;Database=film_db; default command timeout=120; SslMode=none"
  }


///////////// Program.cs içerisinde
using Microsoft.EntityFrameworkCore;
using movie_db.Models;

var connetionString = builder.Configuration.GetConnectionString("DefaultConnection");
builder.Services.AddDbContext<KitapDbContext>(options => options.UseMySql(connetionString, ServerVersion.AutoDetect(connetionString)));


///////////// Controller dosyası içerisinde
Dependency injection yap

using db_conn.Models.Entities;

private readonly FilmDbContext db = new FilmDbContext();
    public HomeController(FilmDbContext _db)
    {
        db = _db;
    }
IactionResult içine
var model = db.Filmers.ToList();
return View(model)

/////////////
home>index.cshmtl en aşağı foreach döngüsü yap

@model List<"projeadı".Models.Entities.Filmler>
<ul>
   @foreach (var Film in Model)
    {
     <li><a href=#>@film.Ad</a></li>
    }
</ul>

