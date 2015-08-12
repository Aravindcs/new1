##<font color="red">Identity API v3 (CURRENT) </font>

<p>Gets an authentication token that permits access to the OpenStack services REST API.</p>
<p>Like most OpenStack projects, OpenStack Identity protects its APIs by defining policy rules
based on a role-based access control (RBAC) approach. These rules are stored in a JSON policy
file. The Identity service configuration file,
<a href="http://docs.openstack.org/kilo/config-reference/content/keystone-configuration-file.html">
    <code>keystone.conf</code>
</a>
sets the name and location
of this policy file. For information about Identity API protection, seeIdentity API protection
with role-based access control (RBAC)in the OpenStack Cloud Administrator Guide
</p>

##<font color="red">1. Tokens</font>

<p>Manages tokens.</p>

<table>
    <tr>
         <th>Method</th>
         <th>URI</th>
         <th>Description</th>
    </tr>
    <tr>
         <td>POST</td>
         <td>/v3/auth/tokens</td>
         <td>Authenticates and generates a token.</td>
    </tr>
    <tr>
         <td>GET</td>
         <td>/v3/auth/tokens</td>
         <td>Validates a specified token.</td>
    </tr>
    <tr>
         <td>HEAD</td>
         <td>/v3/auth/tokens</td>
         <td>Validates a specified token.</td>
    </tr>
    <tr>
         <td>DELETE</td>
         <td>/v3/auth/tokens</td>
         <td>Revokes a specified token.</td>
    </tr>
    <tr>
         <td>POST</td>
         <td>/v3/auth/tokens</td>
         <td>Authenticates and generates a token.</td>
    </tr>  
</table>

###<font color="red">1.1 Authenticate</font>

<table>
    <tr>
         <th>Method</th>
         <th>URI</th>
         <th>Description</th>
    </tr>
    <tr>
         <td>POST</td>
         <td>/v3/auth/tokens</td>
         <td>Authenticates and generates a token.</td>
    </tr>
</table>

<p>
Returns a token, if successful. Each REST request requires the inclusion of a specific authorization
token HTTP x-header, defined as X-Auth-Token. Clients obtain X -Auth-Token
and the URL endpoints for other service APIs by supplying their valid credentials to the authentication
service.
A REST interface provides client authentication by using the POST method, with auth/tokens
supplied as the path. The body of the request must include a payload of credentials
including the authentication method and, optionally, the authorization scope. The scope
includes either a project or domain. If you include both project and domain, an HTTP 400
Bad Request results, because a token cannot be simultaneously scoped as both a project
and domain.
</p>

###<font color="red"> Important </font>

<p>
If you do not include the optional scope and the authenticating user has a defined
default project (the default_project_id attribute for the user), that
default project is treated as the preferred authorization scope.
If no default project is defined, the token is issued without an explicit scope of
authorization.
Provide one of the following sets of credentials to authenticate: User ID and password, user
name and password scoped by domain ID or name, user ID and password scoped by
project ID or name with or without domain scope, or token.
The following examples demonstrate authentication requests with different types of credentials
</p>

###<font color="red"> Note </font>

<p>
If scope is included, project id uniquely identifies the project. However,
project name uniquely identifies the project only when used in conjunction
with a domain ID or a domain name.
If the authentication token has expired, a 401 response code is returned.
If the subject token has expired, this call returns a 404 response code.
The Identity API treats expired tokens as not valid tokens.
The deployment determines how long expired tokens are stored.
As the following example responses show, the response to an authentication request returns
the token ID in the X-Subject-Token header instead of in the token data.
If the call has no explicit authorization scope, the response does not contain the catalog,
project, domain, or roles fields. However, the response still uniquely identifies the user.
A token scoped to a project also has both a service catalog and the user's roles applicable
to the project.
A token scoped to a domain also has both a service catalog and the user's roles applicable
to the project.
Optionally, The Identity API implementation might return an authentication attribute
to indicate the supported authentication methods.
For authentication processes that require multiple round trips, The Identity API implementation
might return an HTTP 401 Unauthorized error with additional information for
the next authentication step.
The following examples illustrate several possible HTTP 401 Unauthorized authentication
errors. Other errors like HTTP 403 Forbidden are also possible.
</p>
<p><b>Normal response codes</b>: 201
</p>
<p><b>Error response codes</b>: Unauthorized (401), Bad Request (400), Unauthorized (401), Forbidden
(403), Method Not Allowed (405), Request Entity Too Large (413), Service Unavailable
(503), Not Found (404)
</p>
     
###<font color="red"> 1.1.1. Request</font>

###<font color="red"> Example 1. Authenticate: JSON request</font>

<pre>
 {
"auth": {
"identity": {
"methods": [
"password"
],
"password": {
"user": {
"id": "0ca8f6",
"password": "secretsecret"
}
}
}
}
}     
</pre>

###<font color="red"> Example 2. Authenticate: JSON request</font>

<pre>    
{
"auth": {
"identity": {
"methods": [
"password"
],
"password": {
"user": {
"domain": {
"id": "1789d1"
},
"name": "Joe",
"password": "secretsecret"
}
}
}
}
}
</pre>

###<font color="red"> Example 3. Authenticate: JSON request</font>

<pre>
{
"auth": {
"identity": {
"methods": [
"password"
],
"password": {
"user": {
"domain": {
"name": "example.com"
},
"name": "Joe",
"password": "secretsecret"
}
}
}
}
}
</pre>

###<font color="red">Example 4. Authenticate: JSON request</font>

<pre>
{
"auth": {
"identity": {
"methods": [
"token"
],
"token": {
"id": "e80b74"
}
}
}
}
</pre>

###<font color="red">Example 5. Authenticate: JSON request</font>

<pre>
{
"auth": {
"identity": {
"methods": [
"password"
],
"password": {
"user": {
"id": "0ca8f6",
"password": "secretsecret"
}
}
},
"scope": {
"project": {
"id": "263fd9"
}
}
}
}
</pre>

###<font color="red">Example 6. Authenticate: JSON request</font>

<pre>
 {
"auth": {
"identity": {
"methods": [
"password"
],
"password": {
"user": {
"id": "0ca8f6",
"password": "secretsecret"
}
}
},
"scope": {
"domain": {
"id": "263fd9"
}
}
}
}
</pre>

###<font color="red">Example 7. Authenticate: JSON request</font>

<pre>
{
"auth": {
"identity": {
"methods": [
"password"
],
"password": {
"user": {
"id": "0ca8f6",
"password": "secretsecret"
}
}
},
"scope": {
"project": {
"domain": {
"id": "1789d1"
},
"name": "project-x"
}
}
}
}
</pre>

###<font color="red">Example 8. Authenticate: JSON request</font>

<pre>
{
"auth": {
"identity": {
"methods": [
"password"
],
"password": {
"user": {
"id": "0ca8f6",
"password": "secretsecret"
}
}
},
"scope": {
"project": {
"domain": {
"name": "example.com"
},
"name": "project-x"
}
}
}
}
</pre>

###<font color="red">1.2 Response</font>

###<font color="red">Example 9. Authenticate: JSON response</font>

<pre>
{
"token": {
"expires_at": "2013-02-27T18:30:59.999999Z",
"issued_at": "2013-02-27T16:30:59.999999Z",
"methods": [
"password"
],
"user": {
"domain": {
"id": "1789d1",
"links": {
"self": "http://identity:35357/v3/domains/1789d1"
},
"name": "example.com"
},
"id": "0ca8f6",
"links": {
"self": "http://identity:35357/v3/users/0ca8f6"
},
"name": "Joe"
}
}
}

</pre>

###<font color="red">Example 10. Authenticate: JSON response</font>

<pre>
{
"token": {
"expires_at": "2013-02-27T18:30:59.999999Z",
"issued_at": "2013-02-27T16:30:59.999999Z",
"methods": [
"password"
],
"endpoints": [
{
"links": {
"self": "https://region-a.example.com:35357/v3/endpoints/
130_P"
},
"id": "example-a",
"interface": "public",
"region_id": "region-a.geo-1",
"url": "https://region-a.example.com:35357/v2.0/",
"service_id": "100"
},
{
"links": {
"self": "https://region-a.example.com:35357/v3/endpoints/
example-a"
},
"id": "example-a",
"interface": "public",
"region_id": "region-a.geo-1",
"url": "https://region-a.example.com:35357/v3/",
"service_id": "100"
}
],
"project": {
"domain": {
"id": "1789d1",
"links": {
"self": "http://identity:35357/v3/domains/1789d1"
},
"name": "example.com"
},
"id": "263fd9",
"links": {
"self": "http://identity:35357/v3/projects/263fd9"
},
"name": "project-x"
},
"roles": [
{
"id": "76e72a",
"links": {
"self": "http://identity:35357/v3/roles/76e72a"
},
"name": "admin"
},
{
"id": "f4f392",
"links": {
"self": "http://identity:35357/v3/roles/f4f392"
},
"name": "member"
}
],
"user": {
"domain": {
"id": "1789d1",
"links": {
"self": "http://identity:35357/v3/domains/1789d1"
},
"name": "example.com"
},
"id": "0ca8f6",
"links": {
"self": "http://identity:35357/v3/users/0ca8f6"
},
"name": "Joe"
}
}
}
</pre>

###<font color="red">Example 11. Authenticate: JSON response</font>

<pre>
{
"token": {
"expires_at": "2013-02-27T18:30:59.999999Z",
"issued_at": "2013-02-27T16:30:59.999999Z",
"methods": [
"password"
],
"catalog": [
{
"type": "identity",
"id": "100",
"endpoints": [
{
"links": {
"self": "https://region-a.example.com:35357/v3/
endpoints/130_P"
},
"id": "example-a",
"interface": "public",
"region_id": "region-a.geo-1",
"url": "https://region-a.example.com:35357/v2.0/",
"service_id": "100"
},
{
"links": {
"self": "https://region-a.example.com:35357/v3/
endpoints/example-a"
},
"id": "example-a",
"interface": "public",
"region_id": "region-a.geo-1",
"url": "https://region-a.example.com:35357/v3/",
"service_id": "100"
}
]
}
],
"domain": {
"id": "1789d1",
"links": {
"self": "http://identity:35357/v3/domains/1789d1"
},
"name": "example.com"
},
"roles": [
{
"id": "76e72a",
"links": {
"self": "http://identity:35357/v3/roles/76e72a"
},
"name": "admin"
},
{
"id": "f4f392",
"links": {
"self": "http://identity:35357/v3/roles/f4f392"
},
"name": "member"
}
],
"user": {
"domain": {
"id": "1789d1",
"links": {
"self": "http://identity:35357/v3/domains/1789d1"
},
"name": "example.com"
},
"id": "0ca8f6",
"links": {
"self": "http://identity:35357/v3/users/0ca8f6"
},
"name": "Joe"
}
}
}
</pre>

###<font color="red"> 1.2 Validate token</font>

<table>
    <tr>
         <th>Method</th>
         <th>URI</th>
         <th>Description</th>
    </tr>
    <tr>
         <td>GET</td>
         <td>/v3/auth/tokens</td>
         <td>Validates a specified token.</td>
    </tr>
</table>

<p>Pass your own token in the X-Auth-Token header and the token to be validated in the
X-Subject-Token header. The Identity API returns the same response as when the subject
token was issued by POST /auth/tokens.
</p>
<p><b>Normal response codes</b>: 200
</p>
<p>
<b>Error response codes</b>: Bad Request (400), Unauthorized (401), Forbidden (403), Method
Not Allowed (405), Request Entity Too Large (413), Service Unavailable (503), Not Found
(404)
</p>

###<font color="red"> 1.2.1. Request</font>

<p>This table shows the header parameters for the validate token request:</p>

<table>
    <tr>
         <th>Name</th>
         <th>Type</th>
         <th>Description</th>
    </tr>
    <tr>
         <td>X-Auth-Token</td>
         <td>String(Required)</td>
         <td>A valid authentication token for an administrative user.</td>
    </tr>
    <tr>
         <td>X-Subject-Token</td>
         <td>String(Required)</td>
         <td>The token ID.</td>
    </tr>
</table>

###<font color="red"> Example 12. Validate token: JSON request</font>

<pre>
Headers:
X-Auth-Token: 1dd7e3
X-Subject-Token: c67580
</pre>

<p>This operation does not accept a request body.
</p>

###<font color="red"> 1.2.2 Response</font>

###<font color="red"> Example 13. Validate token: JSON response</font>

<pre>
{
"token": {
"expires_at": "2013-02-27T18:30:59.999999Z",
"issued_at": "2013-02-27T16:30:59.999999Z",
"methods": [
"password"
],
"user": {
"domain": {
"id": "1789d1",
"links": {
"self": "http://identity:35357/v3/domains/1789d1"
},
"name": "example.com"
},
"id": "0ca8f6",
"links": {
"self": "http://identity:35357/v3/users/0ca8f6"
},
"name": "Joe"
}
}
}
</pre>

###<font color="red"> 1.3. Check token</font>

<table>
    <tr>
         <th>Method</th>
         <th>URI</th>
         <th>Description</th>
    </tr>
    <tr>
         <td>HEAD</td>
         <td>/v3/auth/tokens</td>
         <td>Validates a specified token.</td>
    </tr>
</table>

<p>This call is similar to GET /auth/tokens, but no response body is provided, even in the
X-Subject-Token header.
</p>

###<font color="red"> Important</font>

<p>
The Identity API returns the same response as when the subject token was issued
by POST /auth/tokens, even if an error occurs because the token is
not valid. A 204 response indicates that the X-Subject-Token is valid.
</p>
<p>
<b>Normal response codes</b>: 200
</p>
<p>
<b>Error response codes</b>: Bad Request (400), Unauthorized (401), Forbidden (403), Method
Not Allowed (405), Request Entity Too Large (413), Service Unavailable (503), Not Found
(404)
</p>

###<font color="red"> 1.3.1. Request</font>
<p>This table shows the header parameters for the check token request:</p>

<table>
    <tr>
         <th>Name</th>
         <th>Type</th>
         <th>Description</th>
    </tr>
    <tr>
         <td>X-Auth-Token</td>
         <td>String(Required)</td>
         <td>A valid authentication token for an administrative user.</td>
    </tr>
    <tr>
         <td>X-Subject-Token</td>
         <td>String(Required)</td>
         <td>The token ID.</td>
    </tr>
</table>

###<font color="red"> Example 14. Check token: JSON request</font>
<pre>
Headers:
X-Auth-Token: 1dd7e3
X-Subject-Token: c67580
</pre>
<p>This operation does not accept a request body.</p>
 
###<font color="red"> 1.4. Revoke token</font>

<table>
    <tr>
         <th>Method</th>
         <th>URI</th>
         <th>Description</th>
    </tr>
    <tr>
         <td>HEAD</td>
         <td>/v3/auth/tokens</td>
         <td>Revokes a specified token.</td>
    </tr>
</table>

<p>This call is similar to HEAD /auth/tokens, except that the X-Subject-Token token is
immediately not valid (regardless of the expires_at attribute). An additional X-Auth-
Token is not required.
</p>
<p>
<b>Error response codes</b>: Bad Request (400), Unauthorized (401), Forbidden (403), Method
Not Allowed (405), Request Entity Too Large (413), Service Unavailable (503), Not Found
(404)
</p>

###<font color="red"> 1.4.1. Request</font>
<p>This table shows the header parameters for the revoke token request:</p>
<table>
    <tr>
         <th>Name</th>
         <th>Type</th>
         <th>Description</th>
    </tr>
    <tr>
         <td>X-Auth-Token</td>
         <td>String(Required)</td>
         <td>A valid authentication token for an administrative user.</td>
    </tr>
    <tr>
         <td>X-Subject-Token</td>
         <td>String(Required)</td>
         <td>The token ID.</td>
    </tr>
</table>

###<font color="red"> Example 15. Revoke token: JSON request</font>
<pre>
Headers:
X-Auth-Token: 1dd7e3
X-Subject-Token: c67580
</pre>
<p>This operation does not accept a request body.</p>


