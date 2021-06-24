Projects that utilize the Brightmetrics Billing SDK (and perhaps other internal libraries going forward) need our private GitHub NuGet repository added as a package source.

There are two ways to do that.  GitHub has [some documentation](https://docs.github.com/en/free-pro-team@latest/packages/using-github-packages-with-your-projects-ecosystem/configuring-dotnet-cli-for-use-with-github-packages) around using the dotnet CLI generally, but in brief the command to add the GitHub package source is:

```
dotnet nuget add source https://nuget.pkg.github.com/brightmetrics/index.json -n github -u <username> -p <access_token>
```

The access token needs to be generated from your account settings in GitHub (Developer Settings -> Personal Access Tokens), and have the `read:packages` permission.  If you need to _publish_ packages it also needs `write:packages`, which will also enable full `repo` privileges.

[[images/github-repo-personal-access-token.png]]

Alternatively to the CLI, you can add the package source via Visual Studio by going to the `NuGet Package Manager` area of Options, selecting `Package Sources`, and clicking the green plus icon to add a source.  It should be called `github` and the Source url should be `https://nuget.pkg.github.com/brightmetrics/index.json`.  You should be prompted to authenticate, and again the username is your GitHub username and the password is an access token as described above.