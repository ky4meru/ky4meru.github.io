# Delegations
---

If you identify domain computers vulnerable to unconstrained delegation, refer to [this page](/ad/unconstrained/) to leverage them. 

```powershell
# Look for domain computers vulnerable to unconstrained delegation.
./ADSearch.exe --search "(&(objectCategory=computer)(userAccountControl:1.2.840.113556.1.4.803:=524288))" --attributes samaccountname,dnshostname
```