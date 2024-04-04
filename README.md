# splunk-hec-haproxy-oauth
This repo shows an example of HAProxy configs for setting up an OAuth flow with Splunk HEC. As of this initial writing,
this configuration isn't focused on optimal HEC configuration. This is only for how HAProxy could be the broker between
an OAuth system and Splunk HEC.

The goal for a request like this would be to take a step closer to a "zero trust" environment by never having a password
exchange between Splunk HEC and the sending clients. HAProxy will have the Splunk HEC tokens, and then will verify that
clients have correct OAuth tokens to be able to use those tokens.

## Brief overview
HAProxy 2.5 and later versions now include functions to support JSON Web Tokens (JWTs) which are critical to the OAuth 2.0
authorization protocol. Overall the key things to have HAProxy check is that the token:
- has not expired
- is issued by the trusted authentication service
- is meant for this service (in this case sending to Splunk HEC)
- contains necessary permission grants (if needed)

This example config uses Auth0 and detailed explanation of steps are provided by HAProxy [here](https://www.haproxy.com/documentation/haproxy-configuration-tutorials/authentication/oauth-authorization/).

The one additional step is mapping the JWT audience to a Splunk HEC token map. This is one of many ways to handle mapping
multiple applications, clients, and usecases to multiple HEC tokens. This offers further granularity of control.
