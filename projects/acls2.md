## Repository permissions in Sourcegraph

## Solution

The proposed design breaks repository permissions into three interfaces:

```go
// AuthnProvider supplies the current user's canonical ID. The canonical ID is that
// which uniquely identifies the user in the identity provider, the source of truth
// for identity in the deployment environment. The identity provider can be any of
// the following:
//
// * SAML identity provider
// * OpenID Connect identity provider
// * Code host (e.g., GitHub.com, GitLab.com) if it is used to sign into other services
// * Other SSO identity providers like LDAP, etc.
//
// The identity provider should also be the sign-in mechanism for Sourcegraph.
//
// In most cases, the AuthnProvider will just return the current user's Sourcegraph
// username (which will be the same username provided by the identity provider).
type AuthnProvider interface {
    CurrentIdentity(ctx context.Context) (userID string)
}

// AuthzProvider describes which repositories a user 
type AuthzProvider interface {
    // ListRepositories lists the repositories that the specified user has access to.
    // authzID is the user identity the AuthzProvider uses to determine what repo permissions
    // to return.
    ListRepositories(authzID string) []api.RepoURI
    
    // HasRepository returns true if/only if the authzID has access to the specified repo.
    // It can be implemented in terms of ListRepositories, but in some cases it is more
    // efficient (due to API limits, etc.) to implement it separately.
    HasRepository(authzID string, repo api.RepoURI) bool
}

// TODO
//
// Recommend to keep this as simple as possible (don't return an access token for the authzID,
// because it's good to give that responsibility to the AuthzProvider in case API rate limits
// are a concern.
type IdentityToAuthzIDMapper interface {
    // AuthzID should return the authzID to use for the given authzProvider. This will
    // be the identity function in most cases.
    AuthzID(userID string, authzProvider AuthzProvider) (authzID string)
}

```

### Implementation details

Package `authz` will contain the three interface definitions above and also registration functions:

```go
package authz

func RegisterAuthnProvider(p AuthnProvider)
func RegisterAuthzProvider(p AuthzProvider)
func SetIdentityToAuthzIDMapper(m IdentityToAuthzIDMapper)
```
