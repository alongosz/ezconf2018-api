## REST API
## verbs, media types, response formats

Note:
endpoint prefix, headers (our mime types, accepts, XML, JSON, etc.)
Headers: X-Siteaccess


### REST API

- Remote API implemented over HTTP protocol.
- Reflects Public (PHP) API
- Allows manipulating eZ Platform: Content, Content Types, Locations,
Content Relations, Object States, URL aliases, Users and their permissions (Roles and policies).
- Exposes Search Service (via /views endpoint).
- Uses Repository permissions (by default every request is done as Anonymous user).
- Is **NOT** SiteAccess-aware.


### Documentation

- Currently available at:
https://github.com/ezsystems/ezpublish-kernel/blob/v7.1.1/doc/specifications/rest/REST-API-V2.rst

- Will be moved to the Developer Documentation (http://doc.ezplatform.com)


### REST API:

- is available for every eZ Platform installation at the prefix `/api/ezp/v2`,
- supports HTTP methods (verbs):
    - GET,
    - POST,
    - COPY,
    - PATCH,
    - DELETE,
    - PUBLISH,
    - OPTIONS.


- allows to use `X-Http-Method-override` header if a web server doesn't allow all mentioned methods,
- supports uniformly XML and JSON request payloads and responses.
- Expects in most cases HTTP headers:
    - Cookie,
    - X-CSRF-Token (for requests modifying state of the system),
    - Accepts (with expected by a client media type and format) - to control output,
    - Content-Type (with provided by a client media type and format) - to control input.
    - X-Siteaccess (to provide SiteAccess name, if needed).


#### The Simplest REST API HTTP request

```http
GET /api/ezp/v2 HTTP/1.1
Host: www.example.net
```

returns Root response:


```XML
<?xml version="1.0" encoding="UTF-8"?>
<Root media-type="application/vnd.ez.api.Root+xml">
    <content media-type="" href="/api/ezp/v2/content/objects"/>
    <contentByRemoteId media-type="" href="/api/ezp/v2/content/objects{?remoteId}"/>
    <contentTypes media-type="application/vnd.ez.api.ContentTypeInfoList+xml" href="/api/ezp/v2/content/types"/>
    <contentTypeByIdentifier media-type="" href="/api/ezp/v2/content/types{?identifier}"/>
    <contentTypeGroups media-type="application/vnd.ez.api.ContentTypeGroupList+xml" href="/api/ezp/v2/content/typegroups"/>
    <contentTypeGroupByIdentifier media-type="" href="/api/ezp/v2/content/typegroups{?identifier}"/>
    <users media-type="application/vnd.ez.api.UserRefList+xml" href="/api/ezp/v2/user/users"/>
    <usersByRoleId media-type="application/vnd.ez.api.UserRefList+xml" href="/api/ezp/v2/user/users{?roleId}"/>
    <usersByRemoteId media-type="application/vnd.ez.api.UserRefList+xml" href="/api/ezp/v2/user/users{?remoteId}"/>
    <usersByEmail media-type="application/vnd.ez.api.UserRefList+xml" href="/api/ezp/v2/user/users{?email}"/>
    <usersByLogin media-type="application/vnd.ez.api.UserRefList+xml" href="/api/ezp/v2/user/users{?login}"/>
    <roles media-type="application/vnd.ez.api.RoleList+xml" href="/api/ezp/v2/user/roles"/>
    <rootLocation media-type="application/vnd.ez.api.Location+xml" href="/api/ezp/v2/content/locations/1/2"/>
    <rootUserGroup media-type="application/vnd.ez.api.UserGroup+xml" href="/api/ezp/v2/user/groups/1/5"/>
    <rootMediaFolder media-type="application/vnd.ez.api.Location+xml" href="/api/ezp/v2/content/locations/1/43"/>
    <locationByRemoteId media-type="" href="/api/ezp/v2/content/locations{?remoteId}"/>
    <locationByPath media-type="" href="/api/ezp/v2/content/locations{?locationPath}"/>
    <trash media-type="application/vnd.ez.api.Trash+xml" href="/api/ezp/v2/content/trash"/>
    <sections media-type="application/vnd.ez.api.SectionList+xml" href="/api/ezp/v2/content/sections"/>
    <views media-type="application/vnd.ez.api.RefList+xml" href="/api/ezp/v2/views"/>
    <objectStateGroups media-type="application/vnd.ez.api.ObjectStateGroupList+xml" href="/api/ezp/v2/content/objectstategroups"/>
    <objectStates media-type="application/vnd.ez.api.ObjectStateList+xml" href="/api/ezp/v2/content/objectstategroups/{objectStateGroupId}/objectstates"/>
    <globalUrlAliases media-type="application/vnd.ez.api.UrlAliasRefList+xml" href="/api/ezp/v2/content/urlaliases"/>
    <urlWildcards media-type="application/vnd.ez.api.UrlWildcardList+xml" href="/api/ezp/v2/content/urlwildcards"/>
    <createSession media-type="application/vnd.ez.api.UserSession+xml" href="/api/ezp/v2/user/sessions"/>
    <refreshSession media-type="application/vnd.ez.api.UserSession+xml" href="/api/ezp/v2/user/sessions/{sessionId}/refresh"/>
</Root>
```


### Creating REST session

Request:
```http
POST /user/sessions HTTP/1.1
Host: www.example.net
Accept: application/vnd.ez.api.Session+xml
Content-Type: application/vnd.ez.api.SessionInput+xml

<?xml version="1.0" encoding="UTF-8"?>
<SessionInput>
  <login>admin</login>
  <password>secret</password>
</SessionInput>
```


Response:
```http
HTTP/1.1 201 Created
Location: /user/sessions/go327ij2cirpo59pb6rrv2a4el2
Set-Cookie: eZSSID=go327ij2cirpo59pb6rrv2a4el2; domain=.example.net; path=/; expires=Wed, 13-Jan-2021 22:23:01 GMT; HttpOnly
Content-Type: application/vnd.ez.api.Session+xml

<?xml version="1.0" encoding="UTF-8"?>
<Session href="/user/sessions/sessionID" media-type="application/vnd.ez.api.Session+xml">
  <name>eZSSID</name>
  <identifier>go327ij2cirpo59pb6rrv2a4el2</identifier>
  <csrfToken>23lkneri34ijajedfw39orj3j93</csrfToken>
  <User href="/user/users/14" media-type="vnd.ez.api.User+xml"/>
</Session>
```


### Creating REST session with mixed input and output formats

Request:
```http
POST /user/sessions HTTP/1.1
Host: www.example.net
Accept: application/vnd.ez.api.Session+json
Content-Type: application/vnd.ez.api.SessionInput+xml

<?xml version="1.0" encoding="UTF-8"?>
<SessionInput>
  <login>admin</login>
  <password>secret</password>
</SessionInput>
```


Response:
```http
HTTP/1.1 201 Created
Location: /user/sessions/go327ij2cirpo59pb6rrv2a4el2
Set-Cookie: eZSSID=go327ij2cirpo59pb6rrv2a4el2; domain=.example.net; path=/; expires=Wed, 13-Jan-2021 22:23:01 GMT; HttpOnly
Content-Type: application/vnd.ez.api.Session+json

{
  "Session": {
    "name": "eZSSID",
    "identifier": "go327ij2cirpo59pb6rrv2a4el2",
    "csrfToken": "23lkneri34ijajedfw39orj3j93",
    "User": {
      "_href": "/user/users/14",
      "_media-type": "application/vnd.ez.api.User+json"
    }
  }
}
```


### Search Service example

- search Service expects Criterions, which makes request complicated,
- hence, it requires POST and that implies it requires `X-CSRF-Token` header also.


Request:
```http
POST /api/ezp/v2/views HTTP/1.1
Host: www.example.net
Content-Type: application/vnd.ez.api.ViewInput+xml; version=1.1
Accept: application/vnd.ez.api.Version+json

{
    "ViewInput": {
        "identifier": "my-unique-identifier",
        "public": false,
        "LocationQuery": {
            "Criteria": {},
            "FacetBuilders": {},
            "SortClauses": {
                "LocationPriority": "ascending"
            },
            "Filter": {
                "ParentLocationIdCriterion": "2"
            },
            "limit": 10,
            "offset": 0
        }
    }
}
```


Response:
```json
{
    "View": {
        "_media-type": "application/vnd.ez.api.View+json",
        "_href": "/api/ezp/v2/views/subitems-load-location-2",
        "identifier": "subitems-load-location-2",
        "Query": {
            "_media-type": "application/vnd.ez.api.Query+json"
        },
        "Result": {
            "_media-type": "application/vnd.ez.api.ViewResult+json",
            "_href": "/api/ezp/v2/views/subitems-load-location-2/results",
            "count": 20,
            "time": 0.0048329830169677734,
            "timedOut": false,
            "maxScore": null,
            "searchHits": {
                "searchHit": [
                    {
                        "_media-type": "application/vnd.ez.api.searchHit+json",
                        "_score": 0,
                        "_index": "",
                        "value": {
                            "_media-type": "application/vnd.ez.api.value+json",
                            "Location": {
                                "id": 64
                            }
                        }
                    }
                ]
            }
        }
    }
}
```
