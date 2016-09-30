Tenants, Authentication, and Authorization
==========================================
In order to interact with Azure, through whatever interface, you must first
authenticate.  With the xplat CLI, that looks like this:

```bash
$ azure login
info:    Executing command login
info:    To sign in, use a web browser to open the page https://aka.ms/devicelogin. Enter the code <REDACTED> to authenticate.
info:    Added subscription <REDACTED>
info:    Added subscription <REDACTED>
info:    Added subscription <REDACTED>
info:    login command OK
```

But what is happening behind the scenes?  When you open the web page indicated,
you are prompted to enter the code, which is used to verify that this is a
legitimate authentication request from the CLI.  You are then redirected to
a login page.

## Tenants
On this page, your login (e.g. username@domain) is parsed into a username
and domain.  The domain is then used to lookup the appropriate Azure Active
Directory (AAD) *tenant*.  An AAD tenant is similar in some ways, but very
different in others, from an on-premises instance of Active Directory.  Most
importantly for our purposes, it holds the Azure authentication and
authorization information for an organization.

There are occasions (such as creating a Service Principal, see below)
when you will need to know your TenantId, a unique internal designator 
attached to every AAD tenant.  You can find it via
```bash
azure account show
```

Note that AAD tenants are specific to an [environment](environments.md).  You
will not be able to login to AzureChinaCloud, for example, using credentials
from a tenant in AzureCloud.

## Authentication
There are other ways to authenticate.  For example, using the --tenant, 
-u, and -p switches, you can specify a different AAD tenant, username, and
password.  Alternately, you can use the --certificate-file to login using
your private key.

## Authorization

AAD supports [role-based access control (RBAC)](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-what-is/) for resources,
 with three basic roles: Owner (full access), Contributor (as Owner,
but can't grant rights to others) and Reader (read-only access).  As we
shall see later, resources are structured in a hierarchy, and authorization
can be granted at any level of the resource tree.  Unless specifically
overridden at a lower level, authorization is inherited from higher levels.
