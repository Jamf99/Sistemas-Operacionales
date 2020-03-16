# TALLER POWERSHELL 4
## ACCESO A CARACTERÍSTICAS AVANZADAS DE ADMINISTRACIÓN

#### 1. Mostrar una tabla de procesos que incluya únicamente los nombres de los procesos, sus IDs, y si están respondiendo a Windows (la propiedad Responding muestra eso). Haga que la tabla tome el mínimo de espacio horizontal, pero no permita que la información se trunque.

```powershell
Get-Process | ft -Property Name, ID, Responding -Wrap -AutoSize
```

#### 2. Muestre una tabla de procesos que incluya los nombres de los procesos y sus IDs. También incluya columnas para uso de memoria virtual y física; exprese dichos valores en megabytes (MB).

```powershell
Get-Process | ft -Property Name, ID, @{n='VM (MB)'; e={$_.VM / 1MB}}, @{n='PM (MB)'; e={$_.PM / 1MB}}
```

#### 3. Emplee Get-EventLog para mostrar una lista de los logs de eventos disponibles (revise la ayuda para encontrar el parámetro que le permitirá obtener dicha información). Formatee la salida como una tabla que incluya el nombre de despliegue del log y el período de retención. Los encabezados de columna deben ser NombreLog y Per-Retencion.

```powershell
Get-EventLog -List | ft -Property @{n='NombreLog'; e={$_.Log}}, @{n='Per-Retencion'; e={$_.MinimumRetentionDays}} 
```

#### 4. Muestre una lista de servicios, de tal manera que aparezcan agrupados los servicios que están iniciados y los que están detenidos. Los que están iniciados deben aparecer primero.

```powershell
Get-Service | sort status -Descending | ft -GroupBy status
```

#### 5. Mostrar una lista a cuatro columnas de todos los directorios que están en el raíz de la unidad C:

```powershell
dir | fw -col 4
```

#### 6. Cree una lista formateada de todos los archivos .exe del directorio C:\Windows. Debe mostrarse el nombre, la información de versión, y el tamaño del archivo. La propiedad de tamaño se llama length en Powershell, pero para mayor claridad, la columna se debe llamar Tamaño en su listado.

```powershell
dir -path "C:\Windows\*.exe" | fl name,VersionInfo,@{n='Tamano';e={$_.Length}}
```

#### 7. Importe el módulo NetAdapter (empleando el comando Import-Module NetAdapter). Empleando el cmdlet Get-NetAdapter, muestre una lista de adaptadores no virtuales (adaptadores cuya propiedad Virtual sea falsa. El valor lógico falso es representado por Powershell como $False).

```powershell
Get-NetAdapter | where {$_.Virtual -eq $false}
```

#### 8. Importe el módulo DnsClient. Empleando el cmdlet Get-DnsClientCache, muestre una lista de los registros A y AAAA que estén en el caché. Sugerencia: Si el caché está vacío, visite algunos sitios web para poblarlo.

```powershell
get-dnsclientcache -Type A,AAAA
```

#### 9. Genere una lista de todos los archivos .exe del directorio C:\Windows\System32 que tengan más de 5 MB.

```powershell
dir -path "C:\Windows\System32\*.exe" | where {$_.Length -gt 5MB}
```

#### 10. Muestre una lista de parches que sean actualizaciones de seguridad.

```powershell
Get-HotFix | where {$_.Description -eq "Security Update"}
```

#### 11. Muestre una lista de parches que hayan sido instalados por el usuario Administrador, que sean actualizaciones. Si no tiene ninguno, busque parches instalados por el usuario System. Note que algunos parches no tienen valor en el campo Installed By.

```powershell
Get-HotFix | where {$_.installedby -like "*System*" -and $_.description -like "update"}
```

#### 12. Genere una lista de todos los procesos que estén corriendo con el nombre Conhost o Svchost.

```powershell
Get-Process | where {$_.Name -eq "Conhost" -or $_.Name -eq "Svchost"}
```
