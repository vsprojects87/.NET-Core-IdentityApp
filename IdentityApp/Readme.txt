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


