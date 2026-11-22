
## Tâche 1 : RBAC pour Andrew23

**Énoncé :** utiliser RBAC pour autoriser Andrew23 à modifier les informations suivantes concernant les utilisateurs de l'OU Sales :
- Numéro de téléphone portable
- Informations sur le bureau
- CustomAttribute1

### Commandes

**1. Créer l'étendue limitée à l'OU Sales**
```powershell
New-ManagementScope -Name "Sales Users" `
  -RecipientRoot "contoso23.com/Sales" `
  -RecipientRestrictionFilter {RecipientType -eq "UserMailbox"}
```

**2. Créer le rôle personnalisé**
```powershell
New-ManagementRole -Name "SalesUserInfoEditor" -Parent "Mail Recipients"
```

**3. Supprimer les cmdlets inutiles**
```powershell
Get-ManagementRoleEntry "SalesUserInfoEditor\*" |
  Where-Object { $_.Name -notin "Get-User","Set-User","Get-Mailbox","Set-Mailbox" } |
  Remove-ManagementRoleEntry -Confirm:$false
```

**4. Limiter les paramètres autorisés**
```powershell
Set-ManagementRoleEntry "SalesUserInfoEditor\Set-User" `
  -Parameters "Identity","MobilePhone","Office","Confirm","WhatIf"

Set-ManagementRoleEntry "SalesUserInfoEditor\Set-Mailbox" `
  -Parameters "Identity","CustomAttribute1","Confirm","WhatIf"
```

**5. Créer le groupe de rôles et ajouter Andrew23**
```powershell
New-RoleGroup -Name "Sales Info Editors" `
  -Roles "SalesUserInfoEditor" `
  -CustomRecipientWriteScope "Sales Users" `
  -Members "Andrew23"
```

### Vérification

```powershell
Get-ManagementScope "Sales Users" | fl Name,RecipientRoot,Exclusive
Get-ManagementRoleEntry "SalesUserInfoEditor\*" | ft Name,Parameters -Wrap
Get-RoleGroup "Sales Info Editors" | fl Name,Roles,Members
Get-ManagementRoleAssignment -RoleAssignee "Andrew23" |
  Where-Object { $_.Role -eq "SalesUserInfoEditor" } |
  fl Name,Role,RecipientWriteScope,CustomRecipientWriteScope
```

### Test avec le compte d'Andrew23

```powershell
$cred = Get-Credential   # Andrew23@contoso23.com
$s = New-PSSession -ConfigurationName Microsoft.Exchange `
  -ConnectionUri http://Exchange23.contoso23.com/PowerShell/ `
  -Authentication Kerberos -Credential $cred
Import-PSSession $s -CommandName Get-User,Set-User,Get-Mailbox,Set-Mailbox `
  -DisableNameChecking -AllowClobber

# Doit réussir 
Set-User "Sales1" -MobilePhone "514-555-0100" -Office "Bureau 12"
Set-Mailbox "Sales1" -CustomAttribute1 "Test RBAC"

# Doit échouer
Set-User "Sales1" -Title "Director"
Set-User "Sales1" -City "Montreal"
```

| Test | Résultat |
|---|---|
| Les 3 champs sur Sales1 | Réussi |
| `-Title` | `A parameter cannot be found that matches parameter name 'Title'` |
| `-City` | `couldn't be found` |

---

## Tâche 2 : stratégie de carnet d'adresses IT_ABP

**Énoncé :** créer une nouvelle stratégie de carnet d'adresses nommée `IT_ABP` qui utilise :
- Listes d'adresses : `Networking_teachers`
- Carnet d'adresses hors ligne : `IT_OAB`
- Liste d'adresses globale : `Teachers_GAL`
- Liste des salles : `Third_Floor_Rooms`

### Vérifier ce qui existe déjà

```powershell
Get-AddressList | ft Name,RecipientFilter
Get-GlobalAddressList | ft Name,RecipientFilter
Get-OfflineAddressBook | ft Name,AddressLists
```

### Créer les composants



**1. Liste d'adresses `Networking_teachers`**
```powershell
New-AddressList -Name "Networking_teachers" `
  -RecipientContainer "contoso23.com/Teachers" `
  -IncludedRecipients MailboxUsers `
  -ConditionalDepartment "Networking"
```

**2. Liste d'adresses globale `Teachers_GAL`**
```powershell
New-GlobalAddressList -Name "Teachers_GAL" `
  -RecipientContainer "contoso23.com/Teachers"
```

**3. Liste des salles `Third_Floor_Rooms`**
```powershell
New-AddressList -Name "Third_Floor_Rooms" `
  -RecipientFilter {(RecipientDisplayType -eq 'ConferenceRoomMailbox') -and (Office -eq 'Third Floor')}
```

**4. Carnet d'adresses hors ligne `IT_OAB`**
```powershell
New-OfflineAddressBook -Name "IT_OAB" -AddressLists "\Teachers_GAL"
```

**5. Actualiser le contenu des objets**
```powershell
Update-AddressList "Networking_teachers"
Update-AddressList "Third_Floor_Rooms"
Update-GlobalAddressList "Teachers_GAL"
Update-OfflineAddressBook "IT_OAB"
```

### Créer la stratégie

```powershell
New-AddressBookPolicy -Name "IT_ABP" `
  -AddressLists "\Networking_teachers" `
  -OfflineAddressBook "\IT_OAB" `
  -GlobalAddressList "\Teachers_GAL" `
  -RoomList "\Third_Floor_Rooms"
```

### Vérification

```powershell
Get-AddressBookPolicy "IT_ABP" | fl Name,AddressLists,OfflineAddressBook,GlobalAddressList,RoomList
```

---

## Tâche 3 : Windows Server Backup

**Énoncé :** installer la fonctionnalité Windows Server Backup sur Exchange23 et effectuer une sauvegarde de la base de données de boîtes aux lettres DB23.

_À compléter après le lab._
