---
sidebar_position: 3
slug: /guide/user/troubleshooting
---
# Troubleshooting

Fix common problems while working with letme.

## My terminal cannot locate the ``letme`` binary

Locate the letme binary and move it to one of the locations where your operating system looks for binaries or append the new location to your system's list of locations.

For example, if your $PATH variable looks like this:

```bash
$ echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
```
We will be moving the letme binary to one of the directories above.
```bash
$ mv /home/desktop/letme /usr/local/bin
$ letme --version
letme v0.2.0-rc4 (New Horizons) for (darwin/arm64)

You are using the latest version available.
More info: https://github.com/lockedinspace/letme
```
## Cannot list any accounts while performing ``letme list``

- If you are using letme v2.x.x or higher, check that your context is the correct one with: 
```bash
$ letme config
Available contexts and active context marked with *: 
  general
* sampleContext
```
- Ensure that your context configuration is pointing to the correct dynamodb table and it is using the correct AWS profile.

## Cannot obtain credentials from an account ``AccessDenied: not authorized to perform: sts:AssumeRole``

If trying to obtain credentials fails with:
```bash
$ letme obtain pro-landing-zone-ap
Assuming role with the following session name: 'sample-na-user' and context: 'general'
operation error STS: AssumeRole, https response error StatusCode: 403, RequestID: 11889732-d34d-4e31-a451-40857eef7806, api error AccessDenied: User: arn:aws:iam::4002019901:user/sample-na-user is not authorized to perform: sts:AssumeRole on resource: arn:aws:iam::8002019902:role/account-switch-letme
```
Ensure that you are using an MFA device to authenticate your request, double check your the configuration for the context that you are using.

You might be using the wrong session_name or performing the request from outside of your corporate VPN. 

:::info
Reaching this point means that letme is successfully installed but your are not complying with the security measures your AWS administrator has imposed you.
:::