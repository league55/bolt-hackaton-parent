Let's add authorization and storing data in Supabase:
1. User should be able to see the main page and library page without login
    1.2 Should only be able to see profile page when logged in
2. Auth will be based on username & password
3. Provide me with the query for creating the database table
4. Store sensitive config in the .env file
5. Do you need anything from me? 

--- 
Create a secure authentication system with Supabase integration for a web application:

1. Authentication Requirements:
- Implement username/password authentication using Supabase Auth
- Protect the /profile route with authentication middleware
- Allow public access to all other routes
- Redirect unauthenticated users to /login when accessing protected routes
- Set up proper session management and token handling


Please handle properly signup/login flows, redirections and corner cases

All tabs should be shown in the menu.
Profile is fully secure flow and should redirect to login.
Courses library is public for now