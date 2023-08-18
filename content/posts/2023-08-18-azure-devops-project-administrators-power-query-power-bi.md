+++ 
date = 2023-08-18T16:23:00+01:00
title = "Azure DevOps Project Administrators in Power Query / Power BI"
+++

If you need to show Azure DevOps project information in Power BI, you may find that there is no REST API call to directly get Project Administrators based on the project.

By using DevTools in Edge (or Chrome, or another browser), we can see many web requests that can help us find what we want.

And now let's show the Power Query that does all the magic:

```powerquery
let
    pat = "[pat]",
    organization = "[organization]",
    projectId = "[project]",

    requestBody = "{
    ""contributionIds"": [
        ""ms.vss-admin-web.project-admin-overview-delay-load-data-provider""
    ],
    ""dataProviderContext"": {
        ""properties"": {
            ""projectId"": """ & projectId & """,
            ""sourcePage"": {
                ""routeValues"": {
                    ""project"": """ & projectId & """
                }
            }
        }
    }
    }",

    encodedpat = Binary.ToText(Text.ToBinary(" :" & pat), BinaryEncoding.Base64),
    Source = Json.Document(Web.Contents("https://dev.azure.com/" & organization & "/_apis/Contribution/HierarchyQuery?api-version=5.0-preview.1",[Headers=[#"Content-Type"="application/json", #"Authorization"="Basic " & encodedpat], Content=Text.ToBinary(requestBody)])),
    dataProviders = Source[dataProviders],
    #"ms vss-admin-web project-admin-overview-delay-load-data-provider" = dataProviders[#"ms.vss-admin-web.project-admin-overview-delay-load-data-provider"],
    projectAdmins = #"ms vss-admin-web project-admin-overview-delay-load-data-provider"[projectAdmins],
    identities = projectAdmins[identities],
    #"Converted to Table" = Table.FromList(identities, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", {"identityId", "originId", "subjectKind", "metaType", "displayName", "mailAddress", "descriptor", "entityId", "entityType", "sid"}, {"Column1.identityId", "Column1.originId", "Column1.subjectKind", "Column1.metaType", "Column1.displayName", "Column1.mailAddress", "Column1.descriptor", "Column1.entityId", "Column1.entityType", "Column1.sid"})
in
    #"Expanded Column1"

```
