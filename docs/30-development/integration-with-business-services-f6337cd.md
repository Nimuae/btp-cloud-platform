<!-- loiof6337cd6065a42b59579c069256072ec -->

# Integration with Business Services

Application router supports integration with Business Services, which are a flavor of reuse-services.

An SAP Business Service exposes in its binding information in a set of attributes in the VCAP\_SERVICES credentials block that enable application router to serve Business Service UI and/or data.

To access business services, the following applies:

-   The Business Service UI must be stored in HTML5 Application Repository to be accessible from an application router.

-   The Business Service UI must be defined as "public" to be accessible from an application router in a different space than the one from which the UI was uploaded.

-   The Business Service data can be served using two grant types:


    <table>
    <tr>
    <th valign="top">

    Grant Type


    
    </th>
    <th valign="top">

    Description


    
    </th>
    </tr>
    <tr>
    <td valign="top">

    `user_token`


    
    </td>
    <td valign="top">

    The application router performs a token exchange between the login JWT token and the Business Service token, and uses it to trigger a request to the Business Service endpoint.


    
    </td>
    </tr>
    <tr>
    <td valign="top">

    `client_credentials`


    
    </td>
    <td valign="top">

    The application router generates a `client_credentials` token and uses it to trigger a request to the Business Service endpoint.


    
    </td>
    </tr>
    </table>
    

To bind a Business Service instance to the application router, provide the following information in the VCAP\_SERVICES credentials:


<table>
<tr>
<th valign="top">

Information



</th>
<th valign="top">

Mandatory



</th>
<th valign="top">

Description



</th>
</tr>
<tr>
<td valign="top">

sap.cloud.service



</td>
<td valign="top">

Yes



</td>
<td valign="top">

Service name as referenced from `xs-app.json` route and business service prefix, if provided by the Business Service UI.



</td>
</tr>
<tr>
<td valign="top">

sap.cloud.service.alias



</td>
<td valign="top">

No



</td>
<td valign="top">

Short service name alias for user friendly URL business service prefix. Make sure the alias is unique in the context of the application router.



</td>
</tr>
<tr>
<td valign="top">

endpoints



</td>
<td valign="top">

No



</td>
<td valign="top">

One or more endpoints that can be used to access Business Service data.



</td>
</tr>
<tr>
<td valign="top">

html5-apps-repo



</td>
<td valign="top">

No



</td>
<td valign="top">

The `html5-apps-repo.app_host_id` contains one or more `html5-apps-repo` service instance GUIDs that can be used to retrieve Business Service UIs.



</td>
</tr>
<tr>
<td valign="top">

saasregistryenabled



</td>
<td valign="top">

No



</td>
<td valign="top">

Indicates that this Business Service supports SaaS Registry subscription. If provided, the application router returns this Business Service xsappname in the SaaS Registry `getDependencies` callback.



</td>
</tr>
<tr>
<td valign="top">

grant\_type



</td>
<td valign="top">

No



</td>
<td valign="top">

The grant type that should be used to trigger requests to the Business Service. Allowed values: `user_token` \(default\) or `client_credentials`.



</td>
</tr>
</table>

The value of the `endpoints` is an object containing the following properties:


<table>
<tr>
<th valign="top">

Property



</th>
<th valign="top">

Type



</th>
<th valign="top">

Optional



</th>
<th valign="top">

Default



</th>
<th valign="top">

Description



</th>
</tr>
<tr>
<td valign="top">

url



</td>
<td valign="top">

String



</td>
<td valign="top">

 



</td>
<td valign="top">

 



</td>
<td valign="top">

URL to access the Business Service data.



</td>
</tr>
<tr>
<td valign="top">

timeout



</td>
<td valign="top">

Number



</td>
<td valign="top">

X



</td>
<td valign="top">

30000ms



</td>
<td valign="top">

Positive integer representing the maximum wait time for a response \(in milliseconds\) from the Business Service.



</td>
</tr>
</table>

The following example shows how to provide the required information. This information should be provided via the `onBind` hook in the service-broker implementation:

> ### Sample Code:  
> ```java
> "country": [
>    {
>     ...
>     "credentials": {
>      "sap.cloud.service": "com.sap.appbasic.country", 
>      "sap.cloud.service.alias": "country",            
>      "endpoints": {                                   
>       "countryservice": { "url": https://icf-countriesapp-test-service.cfapps.sap.hana.ondemand.com/odata/v2/countryservice"},
>       "countryconfig":  { 
>             "url": https://icf-countriesapp-test-service.cfapps.sap.hana.ondemand.com/rest/v1/countryconfig",
>             "timeout": 120000 
>       }
>      },
>      "html5-apps-repo": {                           
>       "app_host_id": "1bd7c044-6cf4-4c5a-b904-2d3f44cd5569, 1cd7c044-6cf4-4c5a-b904-2d3f44cd54445"
>      },
>      "saasregistryenabled": true,
>      "grant_type": "user_token"
>    ....
> ```

**Related Information**  


[Accessing Business Service Data](accessing-business-service-data-783809d.md "This section describes how the application router accesses the Business Service data.")

[Accessing Business Service UI](accessing-business-service-ui-0f1f92e.md "This section provides information about accessing Business Services UIs that are stored in HTML5 Application Repository.")

[services](services-92741fa.md "Specify options for a service in your application.")

