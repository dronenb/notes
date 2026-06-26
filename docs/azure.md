# Microsoft Azure

## Installing PowerShell Module

This is necessary on macOS:

```pwsh
Install-Module Microsoft.Graph -Scope CurrentUser -Repository PSGallery -Force
```

## Looking Up Permission IDs

Azure has two types of permissions:

- **Application (Role)**: Permissions granted directly to a service principal/application. These are used for app-to-app scenarios where no user is involved.
- **Delegated (Scope)**: Permissions that allow an app to act on behalf of a signed-in user. The user must consent to these permissions.

### Application Permissions (AppRoles)

#### By Permission Name

```pwsh
$graphSp = Get-MgServicePrincipal -Filter "displayName eq 'Microsoft Graph'" -ErrorAction Stop
$permission = $graphSp.AppRoles | Where-Object { $_.Value -eq "Groups.Read.All" }
$permission.Id
```

#### List All Available Application Permissions

```pwsh
$graphSp = Get-MgServicePrincipal -Filter "displayName eq 'Microsoft Graph'" -ErrorAction Stop
$graphSp.AppRoles | Select-Object Id, Value, DisplayName | Sort-Object Value | Format-Table
```

#### Search by Partial Name

```pwsh
$graphSp = Get-MgServicePrincipal -Filter "displayName eq 'Microsoft Graph'" -ErrorAction Stop
$graphSp.AppRoles | Where-Object { $_.Value -like "*Groups*" } | Select-Object Id, Value, DisplayName | Format-Table
```

### Delegated Permissions (Scopes)

#### By Permission Name

```pwsh
$graphSp = Get-MgServicePrincipal -Filter "displayName eq 'Microsoft Graph'" -ErrorAction Stop
$permission = $graphSp.Oauth2PermissionScopes | Where-Object { $_.Value -eq "Groups.Read.All" }
$permission.Id
```

#### List All Available Delegated Permissions

```pwsh
$graphSp = Get-MgServicePrincipal -Filter "displayName eq 'Microsoft Graph'" -ErrorAction Stop
$graphSp.Oauth2PermissionScopes | Select-Object Id, Value, AdminConsentDisplayName | Sort-Object Value | Format-Table
```

#### Search by Partial Name

```pwsh
$graphSp = Get-MgServicePrincipal -Filter "displayName eq 'Microsoft Graph'" -ErrorAction Stop
$graphSp.Oauth2PermissionScopes | Where-Object { $_.Value -like "*Groups*" } | Select-Object Id, Value, AdminConsentDisplayName | Format-Table
```

## Application Consent Policies

This should be used in scenarios where you need a service principal to be able to provide "admin consent" for a particular permission or set of permissions.

Useful references:

- I originally discovered this in this blog post: <https://www.pimwiddershoven.nl/entry/application-consent-policies-to-delegate-admin-consent/>
- Microsoft's documentation is here: <https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/manage-app-consent-policies>
- <https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/custom-consent-permissions#managing-app-consent-policies>
- <https://learn.microsoft.com/en-us/graph/permissions-reference#policyreadwritepermissiongrant>
- <https://learn.microsoft.com/en-us/graph/api/resources/permissiongrantpolicy?view=graph-rest-1.0>

First, you must login as user that exists within a tenant (IE, not a Microsoft Account), and that user needs to have privileges to create these policies. Not sure what least privilege is for this - perhaps `Policy.ReadWrite.PermissionGrant`, but obviously Global Administrator will be sufficient.

### Interacting with Application Consent Policies via PowerShell

List existing application consent policies:

```pwsh
Get-MgPolicyPermissionGrantPolicy | ft Id, DisplayName, Description
```

Create an empty app consent policy:

```pwsh
$policyId = "tf-consent-azure-k8s-role-assigner-graph-limited"
New-MgPolicyPermissionGrantPolicy `
    -Id $policyId `
    -DisplayName $policyId `
    -Description "This application consent policy allows a particular set of service principals to grant consent to permissions necessary for the CaaS platform"
```

Add an include condition for application permissions `Group.Read.All` and `Application.ReadWrite.OwnedBy`:

```pwsh
# Microsoft Graph service principal (resource API)
$graphSp = Get-MgServicePrincipal -Filter "displayName eq 'Microsoft Graph'" -ErrorAction Stop

# Resolve application permission IDs by permission value
$groupReadAll = $graphSp.AppRoles | Where-Object { $_.Value -eq "Group.Read.All" } | Select-Object -First 1
$appReadWriteOwnedBy = $graphSp.AppRoles | Where-Object { $_.Value -eq "Application.ReadWrite.OwnedBy" } | Select-Object -First 1

if (-not $groupReadAll -or -not $appReadWriteOwnedBy) {
    throw "Could not resolve one or both app role IDs from Microsoft Graph."
}

New-MgPolicyPermissionGrantPolicyInclude `
    -PermissionGrantPolicyId $policyId `
    -PermissionType "application" `
    -ResourceApplication $graphSp.AppId `
    -Permissions @($groupReadAll.Id, $appReadWriteOwnedBy.Id) `
    -ClientApplicationIds @("all")
```

Validate includes for the policy:

```pwsh
Get-MgPolicyPermissionGrantPolicyInclude -PermissionGrantPolicyId $policyId |
    Select-Object Id, PermissionType, ResourceApplication, Permissions |
    Format-Table -AutoSize
```

Delete an app consent policy:

```pwsh
Remove-MgPolicyPermissionGrantPolicy -PermissionGrantPolicyId $policyId
```

### Interacting with Application Consent Policies via `az`

Set variables:

```bash
policy_id="tf-consent-azure-k8s-role-assigner-graph-limited"
graph_app_id="00000003-0000-0000-c000-000000000000"
```

List existing policies:

```bash
az rest --method GET \
    --uri "https://graph.microsoft.com/v1.0/policies/permissionGrantPolicies"
```

Create policy:

```bash
az rest --method POST \
    --uri "https://graph.microsoft.com/v1.0/policies/permissionGrantPolicies" \
    --headers "Content-Type=application/json" \
    --body "{
        \"id\": \"$policy_id\",
        \"displayName\": \"$policy_id\",
        \"description\": \"This application consent policy allows a particular set of service principals to grant consent to permissions necessary for the CaaS platform\"
    }"
```

Resolve Microsoft Graph application permission IDs:

```bash
graph_sp=$(az ad sp show --id "$graph_app_id")
group_read_all_id=$(echo "$graph_sp" | jq -r '.appRoles[] | select(.value=="Group.Read.All") | .id')
application_rw_ownedby_id=$(echo "$graph_sp" | jq -r '.appRoles[] | select(.value=="Application.ReadWrite.OwnedBy") | .id')
```

Add include condition:

```bash
az rest --method POST \
    --uri "https://graph.microsoft.com/v1.0/policies/permissionGrantPolicies/$policy_id/includes" \
    --headers "Content-Type=application/json" \
    --body "{
        \"permissionType\": \"application\",
        \"resourceApplication\": \"$graph_app_id\",
        \"permissions\": [\"$group_read_all_id\", \"$application_rw_ownedby_id\"],
        \"clientApplicationIds\": [\"all\"]
    }"
```

Validate includes:

```bash
az rest --method GET \
    --uri "https://graph.microsoft.com/v1.0/policies/permissionGrantPolicies/$policy_id/includes"
```

Delete policy:

```bash
az rest --method DELETE \
    --uri "https://graph.microsoft.com/v1.0/policies/permissionGrantPolicies/$policy_id"
```

### Custom Role to Delegate Consent to Non-Admins

This pattern assumes your application consent policy already exists, then creates a custom Entra role that references that policy and assigns the role to a specific service principal.

### PowerShell

#### Step 1: Connect to Microsoft Graph

```pwsh
Connect-MgGraph -Scopes "Policy.ReadWrite.PermissionGrant,RoleManagement.ReadWrite.Directory,Application.Read.All"
Select-MgProfile -Name "v1.0"
```

#### Step 2: Set the existing policy ID

```pwsh
$policyId = "tf-consent-azure-k8s-role-assigner-graph-limited"
```

#### Step 3: Create custom role definition that references the policy

Example A: App consent (admin consent) only.

```pwsh
$roleBody = @{
    displayName = "Application Consent Operator (Limited Graph)"
    description = "Allows non-admin role holders to grant app consent only for policy-constrained permissions."
    isEnabled = $true
    rolePermissions = @(
        @{
            allowedResourceActions = @(
                "microsoft.directory/applications/basic/update"
                "microsoft.directory/applications/permissions/update"
                "microsoft.directory/servicePrincipals/managePermissionGrantsForAll.$policyId"
            )
        }
    )
}

$role = New-MgRoleManagementDirectoryRoleDefinition -BodyParameter $roleBody
```

Example B: Delegated user consent only.

```pwsh
$roleBody = @{
    displayName = "Delegated Consent Operator (Limited Graph)"
    description = "Allows non-admin role holders to grant delegated user consent only for policy-constrained permissions."
    isEnabled = $true
    rolePermissions = @(
        @{
            allowedResourceActions = @(
                "microsoft.directory/applications/basic/update"
                "microsoft.directory/applications/permissions/update"
                "microsoft.directory/servicePrincipals/managePermissionGrantsForSelf.$policyId"
            )
        }
    )
}

$role = New-MgRoleManagementDirectoryRoleDefinition -BodyParameter $roleBody
```

#### Step 4: Resolve the target service principal

```pwsh
$targetSpAppId = "<target-service-principal-app-id>"
$targetSp = Get-MgServicePrincipal -Filter "appId eq '$targetSpAppId'" -ErrorAction Stop | Select-Object -First 1

if (-not $targetSp) {
    throw "Target service principal not found."
}
```

#### Step 5: Assign custom role to that service principal

```pwsh
New-MgRoleManagementDirectoryRoleAssignment -BodyParameter @{
    roleDefinitionId = $role.Id
    principalId = $targetSp.Id
    directoryScopeId = "/"
}
```

#### Step 6: Validate role assignment

```pwsh
Get-MgRoleManagementDirectoryRoleAssignment -Filter "principalId eq '$($targetSp.Id)'" |
    Select-Object Id, RoleDefinitionId, PrincipalId, DirectoryScopeId |
    Format-Table -AutoSize
```

### az CLI

#### Step 1: Sign in

```bash
az login
```

#### Step 2: Set the existing policy ID

```bash
policy_id="tf-consent-azure-k8s-role-assigner-graph-limited"
```

#### Step 3: Create custom role definition that references the policy

Example A: App consent (admin consent) only.

```bash
role_json=$(az rest --method POST \
    --uri "https://graph.microsoft.com/v1.0/roleManagement/directory/roleDefinitions" \
    --headers "Content-Type=application/json" \
    --body "{
        \"displayName\": \"Application Consent Operator (Limited Graph)\",
        \"description\": \"Allows non-admin role holders to grant consent only for policy-constrained permissions.\",
        \"isEnabled\": true,
        \"rolePermissions\": [
            {
                \"allowedResourceActions\": [
                    \"microsoft.directory/applications/basic/update\",
                    \"microsoft.directory/applications/permissions/update\",
                    \"microsoft.directory/servicePrincipals/managePermissionGrantsForAll.$policy_id\"
                ]
            }
        ]
    }")

role_definition_id=$(echo "$role_json" | jq -r '.id')
```

Example B: Delegated user consent only.

```bash
role_json=$(az rest --method POST \
    --uri "https://graph.microsoft.com/v1.0/roleManagement/directory/roleDefinitions" \
    --headers "Content-Type=application/json" \
    --body "{
        \"displayName\": \"Delegated Consent Operator (Limited Graph)\",
        \"description\": \"Allows non-admin role holders to grant delegated user consent only for policy-constrained permissions.\",
        \"isEnabled\": true,
        \"rolePermissions\": [
            {
                \"allowedResourceActions\": [
                    \"microsoft.directory/applications/basic/update\",
                    \"microsoft.directory/applications/permissions/update\",
                    \"microsoft.directory/servicePrincipals/managePermissionGrantsForSelf.$policy_id\"
                ]
            }
        ]
    }")

role_definition_id=$(echo "$role_json" | jq -r '.id')
```

#### Step 4: Resolve the target service principal

```bash
target_sp_app_id="<target-service-principal-app-id>"
target_sp_object_id=$(az ad sp show --id "$target_sp_app_id" --query id -o tsv)
```

#### Step 5: Assign custom role to that service principal

```bash
az rest --method POST \
    --uri "https://graph.microsoft.com/v1.0/roleManagement/directory/roleAssignments" \
    --headers "Content-Type=application/json" \
    --body "{
        \"roleDefinitionId\": \"$role_definition_id\",
        \"principalId\": \"$target_sp_object_id\",
        \"directoryScopeId\": \"/\"
    }"
```

#### Step 6: Validate role assignment

```bash
az rest --method GET \
    --uri "https://graph.microsoft.com/v1.0/roleManagement/directory/roleAssignments?$filter=principalId%20eq%20'$target_sp_object_id'"
```

### Probe Roles and Associated Policies

The policy association is encoded in role actions such as:

- `microsoft.directory/servicePrincipals/managePermissionGrantsForAll.<policyId>`
- `microsoft.directory/servicePrincipals/managePermissionGrantsForSelf.<policyId>`

#### List custom roles and policy IDs referenced by their actions

```pwsh
$roleDefs = Get-MgRoleManagementDirectoryRoleDefinition -All
$policiesById = @{}
Get-MgPolicyPermissionGrantPolicy -All | ForEach-Object { $policiesById[$_.Id] = $_ }

$rows = foreach ($role in $roleDefs) {
    $actions = @(
        $role.RolePermissions |
            ForEach-Object { $_.AllowedResourceActions } |
            Where-Object { $_ }
    )

    foreach ($action in $actions) {
        if ($action -match 'managePermissionGrantsFor(All|Self)\.(.+)$') {
            $policyId = $matches[2]
            $policy = $policiesById[$policyId]

            [pscustomobject]@{
                RoleDisplayName   = $role.DisplayName
                RoleId            = $role.Id
                GrantMode         = $matches[1]
                PolicyId          = $policyId
                PolicyDisplayName = if ($policy) { $policy.DisplayName } else { '<not found>' }
                Action            = $action
            }
        }
    }
}

$rows | Sort-Object RoleDisplayName, PolicyId | Format-Table -AutoSize
```

#### List assignments and show principal-to-policy linkage

```pwsh
$roleDefs = Get-MgRoleManagementDirectoryRoleDefinition -All
$roleById = @{}
$roleDefs | ForEach-Object { $roleById[$_.Id] = $_ }

$policiesById = @{}
Get-MgPolicyPermissionGrantPolicy -All | ForEach-Object { $policiesById[$_.Id] = $_ }

$assignmentRows = foreach ($a in (Get-MgRoleManagementDirectoryRoleAssignment -All)) {
    $role = $roleById[$a.RoleDefinitionId]
    if (-not $role) { continue }

    $actions = @(
        $role.RolePermissions |
            ForEach-Object { $_.AllowedResourceActions } |
            Where-Object { $_ -match 'managePermissionGrantsFor(All|Self)\.(.+)$' }
    )

    foreach ($action in $actions) {
        $null = $action -match 'managePermissionGrantsFor(All|Self)\.(.+)$'
        $policyId = $matches[2]
        $policy = $policiesById[$policyId]

        [pscustomobject]@{
            PrincipalId        = $a.PrincipalId
            RoleDisplayName    = $role.DisplayName
            GrantMode          = $matches[1]
            PolicyId           = $policyId
            PolicyDisplayName  = if ($policy) { $policy.DisplayName } else { '<not found>' }
            DirectoryScopeId   = $a.DirectoryScopeId
        }
    }
}

$assignmentRows | Sort-Object RoleDisplayName, PrincipalId | Format-Table -AutoSize
```

### Notes

- `ClientApplicationIds` works as a whitelist when you provide specific client app IDs.
- This role pattern is for delegating consent to non-admin principals, constrained by the policy's include/exclude conditions.
- Keep `microsoft.directory/applications/permissions/update` in the role; without it, consent actions are often blocked.
- If you want user-consent delegation (not only admin consent), also add `microsoft.directory/servicePrincipals/managePermissionGrantsForSelf.<policyId>`.
