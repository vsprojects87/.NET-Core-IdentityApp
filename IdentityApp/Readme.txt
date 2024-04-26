Steps to create Identity project

1. ASP.NET CORE MVC

2. Add New Scaffolded item -> identity -> click on three dots(...) -> select _layout in shared folder as layout
	for identity app

3. choose functionality we want login,logout,register

4. give name of dbcontext class (userdefined) AppDBContext

5. give name of user class (userdefined) ApplicationUser

6. add page link in _layout , we have partial view already generated named _LoginPartial we will add that
                <partial name="_LoginPartial" />

7. we can see register and login on web page after running project but we cant access them since they are razor
	pages so we will need to add support for razor pages

8. To do that we will go to program.cs -> before app.run() we will add middleware 'app.MapRazorPages();'

9. in appdbContext add -> builder.ApplyConfiguration(new ApplicationUserEntityConfiguration()); inside 
	onmodelcreating

10. generate class and implement the interface with applicationuser class
	
    internal class ApplicationUserEntityConfiguration : IEntityTypeConfiguration<ApplicationUser>
    {
        public void Configure(EntityTypeBuilder<ApplicationUser> builder)
        {
            builder.Property(x=>x.FirstName).HasMaxLength(255);
            builder.Property(x => x.LastName).HasMaxLength(255);
        }
    }

11. Add ConnectionString in Appsettings.json which is already added by identity but we need to change servername
    Add in appsettings -> TrustServerCertificate=True

12. Add-Migration Update-Database

13. Add textboxes for Firstname and Lastname -> areas-> pages -> account -> register

14. - on register page we are getting all the tag names attributes from inputmodel class
    eg. input.email , input.password

    - this input is modelclass inside the codebehindpage of register where properties for email,password are define
    - this is where we will define out lastname and firstname

15. in register.cshtml.cs -> before email property

            [Required]
            [StringLength(255,ErrorMessage ="Firstname must be 255 characters")]
            [Display(Name = "Firstname")]
            public string Firstname { get; set; }

            [Required]
            [StringLength(255, ErrorMessage = "Lastname must be 255 characters")]
            [Display(Name = "Lastname")]
            public string Lastname { get; set; }   

    in register.cshtml ->

             <div class="form-floating mb-3">
                <input asp-for="Input.Firstname" class="form-control" autocomplete="Firstname" aria-required="true" placeholder="name@example.com" />
                <label asp-for="Input.Firstname">Firstname</label>
                <span asp-validation-for="Input.Firstname" class="text-danger"></span>
            </div>
            <div class="form-floating mb-3">
                <input asp-for="Input.Lastname" class="form-control" autocomplete="Lastname" aria-required="true" placeholder="name@example.com" />
                <label asp-for="Input.Lastname">Lastname</label>
                <span asp-validation-for="Input.Lastname" class="text-danger"></span>
            </div>

16. in Register.cshtml.cs -> in onpostasync() method which will post the data after submit, after Createuser()
    we will add our firstname and lastname to user
                
                var user = CreateUser();

                user.FirstName = Input.Firstname;
                user.LastName = Input.Lastname;

17. (optional) in program.cs file we can go to defaultidentity service and can make emailauthentication false
    by default it asks for email authentication so we can disable that