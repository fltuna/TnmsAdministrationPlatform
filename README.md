# TnmsAdministrationPlatform

## What is this?

It is a common infrastructure for centralized management of administrator information such as permissions.

## Features

### User Data Management

Administrator information for users is centrally managed using a database.

### Node-based Permission Settings

This system adopts node-based permission settings. Nodes have a hierarchical structure, and permissions can be set for each node. This enables flexible access control.

#### Mechanism

In this system, nodes are defined and utilized as follows:

Examples:
- `tnms.admin`
- `tnms.administrationplatform.command.testcommand1`
- `tnms.administrationplatform.command.testcommand2`

You can also use wildcards to set permissions as shown below:
- `*`: Has all permissions
- `tnms.*`: All permissions related to tnms
- `tnms.administrationplatform.*`: All permissions related to AdministrationPlatform
- `tnms.administrationplatform.command.*`: All permissions related to AdministrationPlatform commands

### Built-in Permissions

- `tnms.admin`: Used to determine whether a user can be targeted in the AdministrationPlatform.
  - Users who do not have this permission cannot target users who do have it.

## Usage

### Config

TODO

## Plugin Development

### Dependencies

Install `TnmsAdministrationPlatform.Shared` from NuGet.

```shell
dotnet add package TnmsAdministrationPlatform.Shared
```

### Permission Verification

You can obtain the AdminSystem as follows:

```csharp
private IAdminManager _adminManager = null!;

public void OnAllModulesLoaded()
{
    var adminSystem = _sharedSystem.GetSharpModuleManager().GetRequiredSharpModuleInterface<IAdminManager>(IAdminManager.ModSharpModuleIdentity).Instance;
    _adminManager = adminSystem ?? throw new InvalidOperationException("TnmsAdministrationPlatform is not found! Make sure TnmsAdministrationPlatform is installed!");
}
```

You can verify permissions as follows:

When using wildcards, except for root permissions, the permission string must end with `.*`.

```csharp
if (_adminManager.ClientHasPermission(player, "node.to.check"))

// Example with wildcard
if (_adminManager.ClientHasPermission(player, "node.to.*"))

// Example with root permission
if (_adminManager.ClientHasPermission(player, "*"))
```

For more details, please refer to the code documentation.
