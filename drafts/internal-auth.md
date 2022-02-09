# Internal Authentication and Authorization

Problem statement: I want to deploy an application that uses google sign-in and has role-based access control.

Expounding:

- The application will have a frontend that is deployed to a public CDN (vercel, github pages)
- It also has an API that contains all the business logic. The API is restricted by both authentication and authorization.
   - This means that the user of the API must be able to 1: prove that they are who they say they are (authentication) and 2: prove that they actually have access to the resource they are trying to access (authorization)
- The Application should ONLY be accessible to users of a google organization (ie: someone@qualtrics.com allowed but jake@gmail.com fails)

This differs from a lot of other situations about using OAuth 2 as an Social Sign in provider ([Dotnet](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/social/), etc.)
because we don't want this to be a "social provider" and we don't want it to be "authentication" only. Most social sign-in providers only provide verification that they are who they say they are.
If you want to limit access based on their authentication, you need to run your own.

For example, using the Google Admin panel to configure an OAuth2 Application works, and allows you to choose the application as "internal", 
but it does not let you specify scopes that you would like to protect/assign to users.

Thus, if we want to add authorization as well, then we need to add a third element to do it for us.

## All-in-one solutions

One solution that will let you do it all is Auth0. They will let you combine Authentication as well as Authorization into one service.

For boxworks, this works well because we are fine letting someone else be our Identity Service. Qualtrics might not be okay with that, but we'll have to make sure.

The way this works is as follows:

![auth0 auth pattern](https://images.ctfassets.net/kbkgmx9upatd/7aDWrJRmhPcPH3pemSWpgE/7763fb527d3044624b21b1b68be7fcac/Access_Control_Hero.png)

The only difference is that the `User successfully authenticated` step is fulfilled not with Auth0's internal sign-in database, but with Google
as the identity provider.

This means that when a user is unauthenticated, we redirect them to Auth0, they redirect the user to Google, which sends it back down the path, and Auth0 will add 
"Claims" that signify that the user can access the resource they are attempting to access.

### BoxWorks use-case

For BoxWorks this works very well. Auth0 believes that anyone can sign in to their application, however when I set up Auth0, I only specified a single login provider (google)
and in the Google console I specified that this was for internal uses only (meaning only \*@boxworks.com) users can login. This allows us to use a "social login" system 
as an internal application auth provider.

The idea behind this scales as well to other solutions. One need not use Auth0 to reap the benefits of this solution. If you choose to run [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/),
or [keycloak](https://www.keycloak.org/) or [Ory](https://www.ory.sh/) as your **Authorization** server, you can just point it at an internal-only Google Sign in OAuth2 client
and you get internal authentication and authorization

### Self-Hosting


## Alternatives

Another alternative that feels more hacky, but does work is using google roles

