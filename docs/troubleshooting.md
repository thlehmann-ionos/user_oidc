# user_oidc troubleshooting

## Changing settings asks for a password (user hard-provisioned in Nextcloud)

### Symptom

When changing settings, i.e. adding an app under User -> Settings -> Security -> Devices & sessions I am asked for a password, yet this is a OIDC-backed session. The user was hard-provisioned using [Nextcloud's provisioning API][1].

The config is

```
'user_oidc' => [
  'auto_provision' => false,
  'soft_auto_provision' => false,
],
```


### Solution

Ensure the user was not only provisioned via Nextcloud's API, but also via user_oidc's provisioning API.

Background: the JSConfigHelper may use the user manager to look up a user backend, to test whether password confirmation is possible when needed. If the user is only found in Nextcloud's backend, but not in user_oidc's backend, this test evaluates to true, thus requiring the user to confirm the password.

[1]: https://docs.nextcloud.com/server/latest/admin_manual/configuration_user/user_provisioning_api.html "Nextcloud: User provisioning API"
