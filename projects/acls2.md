# Repository permissions in Sourcegraph

## Customer questionnaire

TODO

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

// AuthzProvider is the source of truth for which repositories a user is authorized to view.
// The user is specified as an authzID, which identifies the user to the AuthzProvider.
// In most cases, authzID is equivalent
// to the userID supplied by the AuthnProvider, but in some cases, the authorization source of
// truth has its own internal definition of identity which must be mapped from the authentication
// source of truth. The IdentityToAuthzID interface handles this mapping. Examples of
// authorization providers include the following:
//
// * Code host
// * LDAP groups
// * SAML identity provider (via SAML groups)
//
// In most cases, the code host is the source of truth for repository permissions and therefore
// the code host is the authorization provider.
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

// IdentityToAuthzIDMapper maps canonical user IDs (provided by the AuthnProvider) to AuthzProvider
// IDs. In other words, it maps identity provider usernames to authorization provider usernames.
//
// In most cases, the identity provider username should be equivalent to the authorization provider
// username. However, it is not guaranteed. For instance, some code hosts may have a different
// internal username than the username supplied by the SSO login service.
//
// It is recommended to keep this interface as simple as possible. E.g., don't return an access token for the authzID,
// because it's good to give that responsibility to the AuthzProvider in case API rate limits
// are a concern.
type IdentityToAuthzIDMapper interface {
    // AuthzID should return the authzID to use for the given authzProvider. This will
    // be the identity function in most cases.
    AuthzID(userID string, authzProvider AuthzProvider) (authzID string)
}

```

Package `authz` will contain the three interface definitions above and also registration functions:

```go
package authz

func RegisterAuthnProvider(p AuthnProvider)
func RegisterAuthzProvider(p AuthzProvider)
func RegisterIdentityToAuthzIDMapper(m IdentityToAuthzIDMapper)
```

Package `conf` contains the configuration types for code hosts and identity providers. The
following fields will be added:

```go
type GitHubConnection struct {
    ...
    AuthzProvider	bool
    ...
}


type GitLabConnection struct {
    ...
    AuthzProvider	bool
    ...
}


TODO

```

### Scenarios

Let's test out the design in a few scenarios:

#### GitLab + SAML
