# ACCESO A CARACTERÍSTICAS AVANZADAS DE ADMINISTRACIÓN

## ASPECTOS BÁSICOS DE WMI Y CIM

##### 1. ¿Cuál clase puede emplearse para consultar la dirección IP de un adaptador de red? ¿Posee dicha clase algún método para liberar un préstamo de dirección (lease) DHCP?

```powershell
Get-CimInstance win32_networkadapterconfiguration | Select-Object IP
```
No posee un método para librerar un préstamo de dirección DHCP, pero si encuentra los adaptadores.

##### 2. Despliegue una lista de parches empleando WMI (Microsoft se refiere a los parches con el nombre **quick-fix engineering**). Es diferente el listado al que produce el cmdlet ``Get-Hotfix``?

```powershell
Get-WmiObject win32_quickfixengineering
```
Es el mismo listado que produce el cmdlet ``Get-Hotfix``.

##### 3. Empleando WMI, muestre una lista de servicios, que incluya su status actual, su modalidad de inicio, y las cuentas que emplean para hacer login.

```powershell
Get-wmiobject win32_service | select-object status, startmode, systemname
```

##### 4. Empleando cmdlets de CIM, liste todas las clases del namespace ``SecurityCenter2``, que tengan **product** como parte del nombre.

```powershell
get-cimclass -Namespace root/securitycenter2 | where cimclassName -like "*product*"
```

##### 5. Empleando cmdlets de CIM, y los resultados del ejercicio anterior, muestre los nombres de las aplicaciones antispyware instaladas en el sistema. También puede consultar si hay productos antivirus instalados en el sistema.

Para antispyware:
```powershell
get-ciminstance -Namespace root/securitycenter2 -ClassName antispywareproduct
```
Para antivirus:
```powershell
get-ciminstance -Namespace root/securitycenter2 -ClassName antivirusproduct
```
