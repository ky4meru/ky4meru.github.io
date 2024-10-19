# Trust Relationships
---

If you discover trust relationships, you could [abuse](/ad/trust/) them to lateralize on trusted and/or trusting domains.

```powershell
.\ADSearch.exe --search "(objectCategory=trustedDomain)" --domain $domain --attributes distinguishedName,name,flatName,trustDirection
Get-DomainTrust -Domain $domain
```